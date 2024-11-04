`React.memo` เป็น Higher-Order Component (HOC) ใน React ที่ช่วยเพิ่มประสิทธิภาพให้กับ component โดยการทำให้ component ที่ถูกห่อด้วย `React.memo` render ใหม่เฉพาะเมื่อ props ที่ถูกส่งเข้ามามีการเปลี่ยนแปลงจริงๆ เท่านั้น ซึ่งจะช่วยลดการ render ซ้ำที่ไม่จำเป็น ทำให้แอปพลิเคชันทำงานได้รวดเร็วขึ้นในบางกรณี

### วิธีการทำงานของ `React.memo`
`React.memo` จะทำการตรวจสอบ props ที่ส่งเข้ามาใน component และถ้า props ที่เข้ามาไม่มีการเปลี่ยนแปลง มันจะไม่ทำการ re-render component นั้นใหม่ แม้ว่า parent component จะถูก re-render ก็ตาม

ตัวอย่างเช่น:

```javascript
import React from 'react';

const MyComponent = React.memo(({ value }) => {
  console.log('Rendering MyComponent');
  return <div>{value}</div>;
});

export default MyComponent;
```

ในตัวอย่างนี้ ถ้า `MyComponent` ถูกเรียกใช้งานโดยที่ props `value` ไม่มีการเปลี่ยนแปลง `MyComponent` จะไม่ถูก render ใหม่ ดังนั้น คำว่า `"Rendering MyComponent"` จะไม่ปรากฏใน console เมื่อไม่มีการเปลี่ยนแปลงค่า `value`

### ตัวอย่างการใช้งาน `React.memo`

สมมุติว่าเรามี component ชื่อ `ChildComponent` ที่ได้รับ props จาก `ParentComponent` และ `ParentComponent` มีการเปลี่ยนแปลงสถานะบางอย่างที่ไม่ได้เกี่ยวข้องกับ `ChildComponent` ถ้าเราต้องการให้ `ChildComponent` render ใหม่เฉพาะเมื่อ props ที่ได้รับมีการเปลี่ยนแปลง เราสามารถใช้ `React.memo` ได้ ดังนี้:

```javascript
import React, { useState } from 'react';

const ChildComponent = React.memo(({ count }) => {
  console.log('Rendering ChildComponent');
  return <div>Count: {count}</div>;
});

const ParentComponent = () => {
  const [count, setCount] = useState(0);
  const [text, setText] = useState('');

  return (
    <div>
      <ChildComponent count={count} />
      <button onClick={() => setCount(count + 1)}>Increment Count</button>
      <input
        value={text}
        onChange={(e) => setText(e.target.value)}
        placeholder="Type something"
      />
    </div>
  );
};

export default ParentComponent;
```

ในตัวอย่างนี้:
- เมื่อกดปุ่ม `Increment Count` ค่า `count` จะเปลี่ยน และ `ChildComponent` จะถูก render ใหม่ เพราะ props `count` เปลี่ยน
- แต่เมื่อเปลี่ยนข้อความในช่อง `input` ค่า `text` จะเปลี่ยนและทำให้ `ParentComponent` render ใหม่ แต่ `ChildComponent` จะไม่ render ใหม่ เพราะ props `count` ไม่มีการเปลี่ยนแปลง

### เหมาะกับการใช้งานเมื่อไร
การใช้ `React.memo` เหมาะกับ component ที่:
1. **ได้รับ props ที่ไม่ค่อยเปลี่ยนแปลงบ่อย**: ถ้า props ของ component นั้นไม่เปลี่ยนแปลงบ่อย ๆ การใช้ `React.memo` จะช่วยลดการ render ซ้ำที่ไม่จำเป็น
2. **ทำงานที่มีความซับซ้อน**: ถ้า component มีการประมวลผลที่ซับซ้อนหรือมีการทำงานหลายขั้นตอน การลดการ render ที่ไม่จำเป็นจะช่วยเพิ่มประสิทธิภาพของแอปพลิเคชัน
3. **มีการ re-render ของ parent component บ่อย ๆ**: ถ้า parent component มีการเปลี่ยนแปลงสถานะหรือ re-render บ่อย ๆ แต่ props ที่ส่งไปให้ child component นั้นไม่ได้เปลี่ยนแปลง `React.memo` จะช่วยลดการ render ของ child component ที่ไม่จำเป็นได้

### ข้อควรระวังในการใช้ `React.memo`
- `React.memo` เหมาะกับ component ที่มี props แบบ primitive (เช่น string, number, boolean) เพราะการเปรียบเทียบจะเร็วกว่า
- หาก props เป็น `object` หรือ `array` การเปรียบเทียบใน `React.memo` จะเป็นการเปรียบเทียบโดยอิงตาม reference (ไม่ใช่การเปรียบเทียบค่าแบบ deep comparison) ซึ่งอาจทำให้ component re-render แม้ว่าเนื้อหาภายในจะไม่เปลี่ยนแปลง หากจำเป็นต้องทำ deep comparison สามารถใช้ `React.memo` พร้อมกับ custom comparison function ได้

ตัวอย่างการใช้ custom comparison function ใน `React.memo`:

```javascript
const MyComponent = React.memo(
  ({ user }) => {
    console.log('Rendering MyComponent');
    return <div>{user.name}</div>;
  },
  (prevProps, nextProps) => {
    return prevProps.user.id === nextProps.user.id;
  }
);
```

ในตัวอย่างนี้ `MyComponent` จะถูก render ใหม่เฉพาะเมื่อ `user.id` มีการเปลี่ยนแปลงเท่านั้น แม้ว่า `user` จะเป็น object ที่มี reference เปลี่ยนแปลง
