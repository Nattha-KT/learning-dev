**Path Aliases** คือการตั้งค่าชื่อทางลัด (alias) สำหรับเส้นทางไฟล์ (file paths) ในโปรเจกต์ของคุณ เพื่อให้คุณสามารถอ้างอิงไฟล์ในโฟลเดอร์ต่าง ๆ ได้ง่ายขึ้น โดยไม่ต้องใช้ relative paths ที่ซับซ้อน เช่น `../../components/Button` เมื่อคุณต้องการนำเข้าโมดูลจากไฟล์ที่อยู่ในหลายระดับของโฟลเดอร์

### ตัวอย่างการใช้ Path Aliases:
แทนที่จะเขียนแบบนี้:
```ts
import Button from '../../../components/Button';
```

คุณสามารถกำหนด path alias ในไฟล์ `tsconfig.json` ของ TypeScript เพื่อให้สามารถเขียนได้แบบนี้แทน:
```ts
import Button from '@components/Button';
```

### วิธีตั้งค่า Path Aliases ใน `tsconfig.json`
1. เปิดไฟล์ `tsconfig.json`
2. เพิ่มการตั้งค่า `paths` ในส่วนของ `compilerOptions`:

```json
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@components/*": ["src/components/*"],
      "@utils/*": ["src/utils/*"]
    }
  }
}
```

ในตัวอย่างนี้:
- `@components/*` หมายถึงทุกไฟล์ที่อยู่ในโฟลเดอร์ `src/components/`
- `@utils/*` หมายถึงทุกไฟล์ที่อยู่ในโฟลเดอร์ `src/utils/`

หลังจากตั้งค่า path aliases แบบนี้ คุณสามารถนำเข้าไฟล์ได้ง่ายขึ้น:

```ts
import Button from '@components/Button';
import { calculate } from '@utils/helpers';
```

### ประโยชน์ของ Path Aliases
- **ทำให้โค้ดอ่านง่ายขึ้น**: ลดความยุ่งยากจากการใช้ relative paths หลายระดับ (`../../../`)
- **ลดข้อผิดพลาด**: เมื่อย้ายไฟล์ไปยังตำแหน่งใหม่ Path Aliases ช่วยลดปัญหาเรื่องการแก้ไขเส้นทาง
- **ช่วยให้โครงสร้างโปรเจกต์ชัดเจนขึ้น**: คุณสามารถตั้งชื่อ alias ให้สอดคล้องกับโครงสร้างโฟลเดอร์

### การใช้งานใน Storybook
ถ้าคุณใช้ Path Aliases ใน `tsconfig.json` คุณจำเป็นต้องใช้ปลั๊กอิน `vite-tsconfig-paths` ใน Storybook (ถ้าใช้ Vite) เพื่อให้ Storybook สามารถ resolve paths เหล่านั้นได้