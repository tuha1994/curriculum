### Giới thiệu

Hãy thực hành! Việc testing này thực sự không khó đến vậy, nhưng *vẫn* còn khá mới. Cách duy nhất để cảm thấy quen thuộc là dành thời gian thực hiện nó.

### Bài tập

<div class="lesson-content__panel" markdown="1">

Hãy viết test cho những yêu cầu sau, rồi làm cho các test pass! Hãy nhớ rằng Jest không có hỗ trợ ổn định được tích hợp sẵn cho ESM, vì vậy bạn sẽ cần [thiết lập Babel để chuyển đổi Jest ESM/CJS](https://www.theodinproject.com/lessons/node-path-javascript-testing-basics#using-es6-import-statements-with-jest).

1. Một function `capitalize` nhận một string và trả về string đó với ký tự đầu tiên được viết hoa.
1. Một function `reverseString` nhận một string và trả về string đó sau khi đảo ngược.
1. Một object `calculator` chứa các function cho những phép toán cơ bản: `add`, `subtract`, `divide` và `multiply`. Mỗi function cần nhận hai số và trả về phép tính chính xác.
1. Một function `caesarCipher` nhận một string và một shift factor rồi trả về string với mỗi ký tự được "dịch chuyển". Đọc thêm về [cách Caesar cipher hoạt động](https://crypto.interactive-maths.com/caesar-shift-cipher.html).
   1. Đừng quên test việc chuyển vòng từ `z` sang `a`. Ví dụ, `caesarCipher('xyz', 3)` phải trả về `'abc'`.
   1. Đừng quên test việc giữ nguyên chữ hoa chữ thường. Lettercase sau khi dịch chuyển phải theo lettercase ban đầu. Ví dụ, `caesarCipher('HeLLo', 3)` phải trả về `'KhOOr'`.
   1. Đừng quên test dấu câu. Dấu câu, khoảng trắng và các ký tự không phải chữ cái khác phải được giữ nguyên. Ví dụ, `caesarCipher('Hello, World!', 3)` phải trả về `'Khoor, Zruog!'`.
   1. Với bài này, bạn có thể muốn tách function cuối thành một vài function nhỏ hơn. Một khái niệm của Testing là bạn không cần test tường minh *mọi* function mình viết... Chỉ cần test các function public. Vì vậy, trong trường hợp này, bạn chỉ cần test function `caesarCipher` cuối cùng. Nếu nó hoạt động như mong đợi, bạn có thể yên tâm rằng các helper function nhỏ hơn đang làm đúng nhiệm vụ.
1. Một function `analyzeArray` nhận một array các số và trả về một object có các thuộc tính sau: `average`, `min`, `max` và `length`.

   ```javascript
   const object = analyzeArray([1,8,3,4,2,6]);

   // object should equal:
   {
     average: 4,
     min: 1,
     max: 8,
     length: 6
   }
   ```

</div>