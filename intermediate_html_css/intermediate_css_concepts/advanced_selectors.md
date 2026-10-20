### Giới thiệu

Đến giờ, có lẽ bạn đã quen với các selector CSS cơ bản và không gặp khó khăn khi chọn phần tử theo type, class hoặc ID. Nhưng để trở thành một "bác sĩ phẫu thuật" CSS thực thụ, đôi khi bạn cần những công cụ chuyên biệt hơn. Trong bài này, chúng ta sẽ xem các selector CSS nâng cao và chỉ cho bạn cách nhắm đến phần tử một cách chi tiết hơn.

Các selector này đặc biệt hữu ích khi bạn không thể (hoặc không muốn) thay đổi markup HTML.

Có *rất nhiều* selector nâng cao, nên việc đi qua từng selector nằm ngoài phạm vi bài này. Tuy nhiên, chúng ta sẽ tìm hiểu một số selector hữu ích và phổ biến nhất, đồng thời trang bị cho bạn các khái niệm và từ vựng để tự học thêm.

Như mọi khi, hãy thoải mái mở trình soạn thảo code và tự thử nghiệm với các selector này — luyện tập sẽ giúp bạn thành thạo!

### Tổng quan bài học

Phần này trình bày khái quát các chủ đề bạn sẽ học trong bài này.

- Hiểu cách sử dụng selector parent và sibling.
- Nhận biết sự khác nhau giữa pseudo class và pseudo element.
- Tìm hiểu một số pseudo element và pseudo class hữu ích, phổ biến nhất.
- Tìm hiểu các cách khác nhau để chọn một attribute hoặc các phần của nó.

### Combinator của phần tử con và sibling

Hãy xem thêm một số cách để truy cập các phần tử khác nhau *mà không* cần tham chiếu đến class của chúng. Dưới đây là ba selector mới giúp làm điều đó.

- `>` - combinator phần tử con
- `+` - combinator sibling kế tiếp
- `~` - combinator sibling tổng quát

Chúng ta sẽ xử lý một số ví dụ thực tế bằng markup mẫu này.

```html
<main class="parent">
  <div class="child group1">
    <div class="grand-child group1"></div>
  </div>
  <div class="child group2">
    <div class="grand-child group2"></div>
  </div>
  <div class="child group3">
    <div class="grand-child group3"></div>
  </div>
</main>
```

Đến giờ, có lẽ bạn khá quen với việc viết rule bằng descendant combinator đã học trong [Intro to CSS](https://www.theodinproject.com/lessons/foundations-intro-to-css). Ví dụ, nếu muốn chọn tất cả div `child` và `grand-child` bên trong `main`, chúng ta có thể viết:

```css
main div {
  /* Our cool CSS */
}
```

Nhưng nếu muốn cụ thể hơn và chỉ chọn các div `child` hoặc `grand-child` thì sao? Đó là lúc child combinator `>` trở nên hữu ích. Khác với descendant combinator, nó chỉ chọn các phần tử con trực tiếp.

```css
/* The divs with the class "child" will get selected by this */
main > div {
  /* Our cool CSS */
}

/* The divs with the class "grand-child" will get selected by this */
main > div > div {
  /* More cool CSS */
}
```

Nói theo cách khác, child selector sẽ chọn một phần tử nằm thấp hơn một cấp thụt lề. Để chọn phần tử nằm cạnh (ngay sau) phần tử mục tiêu, hoặc ở cùng cấp thụt lề, chúng ta có thể dùng next-sibling combinator `+`.

```css
/* Only the div with the classes "child group2" will get selected by this */
.group1 + div {
  /* Our cool CSS */
}

/* Only the div with the classes "child group3" will get selected by this */
.group1 + div + div {
  /* More cool CSS */
}
```

Cuối cùng, nếu muốn chọn tất cả sibling đứng sau một phần tử chứ không chỉ phần tử đầu tiên, chúng ta có thể dùng general sibling combinator `~`.

```css
/* All of .group1's div siblings - in this case the 2nd and 3rd .child divs, will get selected by this */
.group1 ~ div {
  /* Our cool CSS */
}
```

Cũng giống descendant combinator, các selector này không có quy tắc specificity đặc biệt — điểm specificity của chúng chỉ được tạo thành từ các phần cấu thành.

[Bài viết MDN về combinator](https://developer.mozilla.org/en-US/docs/Learn/CSS/Building_blocks/Selectors/Combinators) này cung cấp phần tổng quan tốt nếu bạn muốn tìm hiểu thêm.

### Pseudo-selector

Trước khi đi sâu vào pseudo-selector, cần nói nhanh về sự khác nhau giữa [pseudo-class và pseudo-element](https://developer.mozilla.org/en-US/docs/Learn/CSS/Building_blocks/Selectors/Pseudo-classes_and_pseudo-elements). Pseudo-class selector có một dấu hai chấm ở đầu và là một cách khác để nhắm đến các phần tử đã tồn tại trong HTML. Pseudo-element có hai dấu hai chấm ở đầu và được dùng để nhắm đến các phần tử vốn *không* thường tồn tại trong markup. Nếu điều này chưa có ý nghĩa ngay, đừng lo — chúng ta sẽ khám phá một số ví dụ bên dưới.

### Pseudo-class

Pseudo-class cung cấp những cách khác nhau để nhắm đến các phần tử trong HTML. Có khá nhiều pseudo-class và chúng thuộc một vài nhóm khác nhau. Một số dựa trên vị trí hoặc cấu trúc của chúng trong HTML. Một số khác dựa trên trạng thái của một phần tử cụ thể hoặc cách người dùng hiện tương tác với nó. Có quá nhiều pseudo-class để trình bày chi tiết ở đây, nhưng chúng ta sẽ xem một số loại hữu ích nhất. Pseudo-class có cùng specificity với class thông thường (0, 0, 1, 0). Cũng như class thông thường, hầu hết pseudo-class có thể được nối chuỗi.

<div class="lesson-note lesson-note--tip" markdown="1">

#### Tính ký hiệu specificity của CSS

(0,0,1,0) ở trên là ký hiệu dùng để tính specificity. Để tìm hiểu thêm về cách hoạt động, hãy xem qua phần "Calculating CSS Specificity Value" trong [bài viết về CSS Specificity](https://css-tricks.com/specifics-on-css-specificity/#aa-calculating-css-specificity-value) này.

</div>

Như mọi khi, đừng quên xem [tài liệu MDN về pseudo-class](https://developer.mozilla.org/en-US/docs/Web/CSS/Pseudo-classes) để có cái nhìn đầy đủ về những gì đang có.

#### Pseudo-class động và hành động người dùng

Những pseudo-class hữu ích này có thể khiến trang của bạn trở nên năng động và tương tác hơn.

[`:focus`](https://css-tricks.com/almanac/pseudo-selectors/f/focus/) áp dụng cho phần tử hiện đang được người dùng chọn, bằng con trỏ hoặc bằng bàn phím.

[`:hover`](https://css-tricks.com/almanac/pseudo-selectors/h/hover/) tác động đến bất kỳ thứ gì nằm dưới con trỏ chuột của người dùng. Nó có thể được dùng để làm các button và link nổi bật hơn, cho người dùng thấy chúng có thể tương tác, hoặc để kích hoạt menu thả xuống.

[`:active`](https://css-tricks.com/almanac/pseudo-selectors/a/active/) áp dụng cho các phần tử đang được click và đặc biệt hữu ích để cho người dùng phản hồi rằng hành động của họ đã có tác dụng. Đây là một lựa chọn tuyệt vời để tạo phản hồi "có cảm giác" hơn cho button và các phần tử tương tác khác.

Bạn đã bao giờ thắc mắc tại sao link có màu xanh nhưng chuyển sang tím khi được click trong HTML không có style chưa? Đó là vì trình duyệt triển khai style đó theo mặc định. Để tự tạo style cho link, hãy tận dụng pseudo-class [`:link`](https://css-tricks.com/almanac/pseudo-selectors/l/link/) và [`:visited`](https://css-tricks.com/almanac/pseudo-selectors/v/visited/). Một phiên bản đơn giản hóa của style trình duyệt mặc định có thể trông như sau:

```css
  /* This rule will apply to all links */
  a {
    text-decoration: underline;
  }

  /* This will apply to unvisited links */
  a:link {
    color: blue;
  }

  /* And you guessed it, this applies to all links the user has clicked on */
  a:visited {
    color: purple;
  }
```

#### Pseudo-class cấu trúc

Pseudo-class cấu trúc là một cách mạnh mẽ để chọn phần tử dựa trên vị trí của chúng trong DOM.

[`:root`](https://css-tricks.com/almanac/pseudo-selectors/r/root/) là một class đặc biệt đại diện cho cấp cao nhất của tài liệu — phần tử duy nhất không có phần tử cha. Nhìn chung khi làm việc với web, nó tương đương phần tử `html`, nhưng có [một vài khác biệt nhỏ giữa `:root` và `html`](https://stackoverflow.com/questions/15899615/whats-the-difference-between-css3s-root-pseudo-class-and-html).

`:root` thường là nơi bạn đặt các rule CSS "toàn cục" muốn có ở mọi nơi — chẳng hạn custom properties và biến CSS, hoặc các rule như `box-sizing: border-box;`.

[`:first-child`](https://css-tricks.com/almanac/pseudo-selectors/f/first-child/) và [`:last-child`](https://css-tricks.com/almanac/pseudo-selectors/l/last-child/) sẽ khớp với các phần tử là sibling đầu tiên hoặc cuối cùng.

Tương tự, [`:empty`](https://css-tricks.com/almanac/pseudo-selectors/e/empty/) sẽ khớp với phần tử hoàn toàn không có phần tử con, còn [`:only-child`](https://css-tricks.com/almanac/pseudo-selectors/o/only-child/) sẽ khớp với phần tử không có sibling nào.

Để có cách tiếp cận linh hoạt hơn, chúng ta có thể dùng [`:nth-child`](https://css-tricks.com/almanac/pseudo-selectors/n/nth-child/). Đây là một pseudo-class linh hoạt với một vài cách sử dụng khác nhau.

```css
  .myList:nth-child(5) {/* Selects the 5th element with class myList */}

  .myList:nth-child(3n) { /* Selects every 3rd element with class myList */}

  .myList:nth-child(3n + 3) { /* Selects every 3rd element with class myList, beginning with the 3rd */}

  .myList:nth-child(even) {/* Selects every even element with class myList */}
```

### Pseudo-element

Trong khi pseudo-class cho chúng ta một cách tương tác thay thế với các phần tử HTML dựa trên trạng thái hoặc cấu trúc, pseudo-element mang tính trừu tượng hơn. Chúng cho phép tác động đến những phần HTML vốn hoàn toàn không phải là phần tử. Các phần tử đặc biệt này có cùng specificity với phần tử thông thường (0, 0, 0, 1). Có nhiều pseudo-element hữu ích có thể được dùng theo vô số cách sáng tạo.

[`::marker`](https://css-tricks.com/almanac/pseudo-selectors/m/marker/) cho phép bạn tùy chỉnh style của dấu đầu dòng hoặc số trên các phần tử `<li>`.

[`::first-letter`](https://css-tricks.com/almanac/pseudo-selectors/f/first-letter/) và [`::first-line`](https://css-tricks.com/almanac/pseudo-selectors/f/first-line/) cho phép bạn (đúng như tên gọi!) tạo style đặc biệt cho chữ cái hoặc dòng đầu tiên của một đoạn văn bản.

[`::selection`](https://css-tricks.com/almanac/pseudo-selectors/s/selection/) cho phép bạn thay đổi phần highlight khi người dùng chọn văn bản trên trang.

[`::before` và `::after`](https://css-tricks.com/almanac/pseudo-selectors/b/after-and-before/) cho phép chúng ta thêm phần tử bổ sung vào trang bằng CSS thay vì HTML. Dùng chúng để trang trí văn bản theo nhiều cách là một trường hợp sử dụng phổ biến.

```html
<style>
  .emojify::before {
    content: '😎 😄 🤓';
}

  .emojify::after {
    content: '🤓 😄 😎';
}
</style>

<body>
  <div> Let's <span class="emojify">emojify</span>this span!</div>
</body>
```

Dùng pseudo-element theo cách này sẽ cho chúng ta kết quả:

Let's 😎 😄 🤓 emojify 🤓 😄 😎 this span!

 Còn rất nhiều pseudo-element khác! Hãy xem nhanh [tài liệu pseudo-element](https://developer.mozilla.org/en-US/docs/Web/CSS/Pseudo-elements) để biết đầy đủ những gì có thể làm.

### Attribute selector

Công cụ cuối cùng chúng ta sẽ thêm vào hộp công cụ là attribute selector. Hãy nhớ rằng attribute là bất cứ thứ gì nằm trong tag mở của một phần tử HTML — chẳng hạn `src='picture.jpg'` hoặc `href="www.theodinproject.com"`.

Vì chúng ta tự viết giá trị cho attribute, cần một hệ thống linh hoạt hơn để có thể nhắm đến các giá trị cụ thể.

Attribute selector có cùng specificity với class và pseudo-class (0, 0, 1, 0).

Hãy xem một số ví dụ sử dụng cơ bản.

- `[attribute]` - selector tổng quát này sẽ chọn mọi thứ có attribute đã cho. Giá trị của nó không quan trọng.
- `selector[attribute]` - chúng ta có thể tùy chọn kết hợp attribute selector với các loại selector khác, chẳng hạn class hoặc element selector.
- `[attribute="value"]` - để cụ thể hơn, chúng ta có thể dùng `=` để khớp một attribute với một giá trị cụ thể.

```css
  [src] {
    /* This will target any element that has a src attribute. */
  }

  img[src] {
    /* This will only target img elements that have a src attribute. */
  }

  img[src="puppy.jpg"] {
    /* This will target img elements with a src attribute that is exactly "puppy.jpg" */
  }
```

Đôi khi chúng ta cần một cách tổng quát hơn để truy cập các attribute này. Ví dụ, có thể chúng ta chỉ quan tâm đến các phần tử `img` có giá trị attribute `src` kết thúc bằng `.jpg`. Với những trường hợp như vậy, chúng ta có một số attribute selector cho phép khớp một phần giá trị của attribute. Nếu từng gặp [regular expression](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions), bạn sẽ thấy các attribute này dùng cú pháp tương tự.

- `[attribute^="value"]` - `^=` sẽ khớp chuỗi từ đầu.
- `[attribute$="value"]` - `$=` sẽ khớp chuỗi từ cuối.
- `[attribute*="value"]` - `*=` selector wildcard sẽ khớp ở bất kỳ vị trí nào trong chuỗi.

```css
[class^='aus'] {
  /* Classes are attributes too!
    This will target any class that begins with 'aus':
    class='austria'
    class='australia'
  */
}

[src$='.jpg'] {
  /* This will target any src attribute that ends in '.jpg':
  src='puppy.jpg'
  src='kitten.jpg'
  */
}

[for*='ill'] {
  /* This will target any for attribute that has 'ill' anywhere inside it:
  for="bill"
  for="jill"
  for="silly"
  for="ill"
  */
}
```

Để xem attribute selector còn có thể làm được gì, chẳng hạn tìm kiếm không phân biệt hoa thường hoặc các chuỗi con được phân tách bằng dấu gạch ngang, hãy xem [tài liệu MDN về attribute selector](https://developer.mozilla.org/en-US/docs/Web/CSS/Attribute_selectors).

### Bài tập

<div class="lesson-content__panel" markdown="1">

1. Hoàn thành [CSS Diner](https://flukeout.github.io/). Bạn có thể đã quen với phần lớn nội dung trong vài bài tập đầu, nhưng luyện tập và ôn lại không bao giờ thừa! Đừng quên đọc các ví dụ và phần giải thích ở bên phải.
1. Đọc [bài viết của Shay Howe về Complex Selectors](https://learn.shayhowe.com/advanced-html-css/complex-selectors/). Bài viết trình bày phần lớn nội dung của bài này chi tiết hơn một chút. Như bài viết đã nêu, đôi khi tác giả dùng một dấu hai chấm thay vì hai dấu cho pseudo-element. Hãy nhớ rằng hiện nay hai dấu hai chấm mới là tiêu chuẩn.
1. Làm [Selectors Assessment](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/Styling_basics/Basic_selectors/Selectors_Tasks) của MDN. Bài này sẽ giúp bạn vận dụng kiến thức mới học về selector!

</div>