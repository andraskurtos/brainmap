---
aliases: 
tags:
  - client
  - 5semester
  - uni
cssclasses:
  - center-images
  - center-titles
  - markdown-preview-view
created: 2024-12-19 18:25
---

# Vizsga gyakorlás

> [!QUESTION]- Írj egy React hookot (useCountdown), ami minden másodpercben újra renderel, és a megadott értékről lefelé számol! Példa a használatára: let count=useCountdown(15);
>
>```ts
>public void useCountDown({initial}:{initial:number}) {
>	let [count, setCount] = useState(initial);
>
>	useEffect( ()=> {
>		if (count<=0) return;
>
>		const interval = setInterval(()=> {
>			setCount((prev)=>prev-1);
>		},1000);
>		
>		return () => clearInterval(interval);
>	}, [count]);
>
>	return count;
>}
>```

```typescript

export function useCountDown({initial}:{initial:number})
{
	let [count, setCount] = useState(initial);

	useEffect(()=>{
		if (count<=0) return;
		
		let interval = setInterval(()=>{
			setCount((prev)=>prev-1);
		},1000);

		return ()=>clearInterval(interval);
	}, [count]);

	return count;
}
```


