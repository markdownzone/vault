```js
const percentDate = (secondsTotal, secondsRem) => Number(100*secondsRem/secondsTotal).toFixed(2) + "%";

const dayP = percentDate(
	DateTime.now().endOf('day').diff(DateTime.now().startOf('day'), 'seconds'),
	DateTime.now().endOf('day').diff(DateTime.now(), 'seconds')
);
const weekP = percentDate(
	DateTime.now().endOf('week').diff(DateTime.now().startOf('week'), 'seconds'), 
	DateTime.now().endOf('week').diff(DateTime.now(), 'seconds')
);
const monthP = percentDate(
	DateTime.now().endOf('month').diff(DateTime.now().startOf('month'), 'seconds'),
	DateTime.now().endOf('month').diff(DateTime.now(), 'seconds')
);
const yearP = percentDate(
	DateTime.now().endOf('year').diff(DateTime.now().startOf('year'), 'seconds'),
	DateTime.now().endOf('year').diff(DateTime.now(), 'seconds')
);
const lifeP = percentDate(
	DateTime.fromISO('2051-07-08T09:10:11.12').diff(DateTime.fromISO('1970-01-02T03:04:05.06'), 'seconds'),
	DateTime.fromISO('2051-07-08T09:10:11.12').diff(DateTime.now(), 'seconds')
);

dv.paragraph(dv.markdownTable(["Scope", "Remaining %"], [["Day", dayP],["Week", weekP], ["Month", monthP], ["Year", yearP], ["Life", lifeP]]))
```