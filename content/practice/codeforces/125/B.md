---
title: "CF 125B - XML ​​đơn giản"
description: "Đầu vào là một văn bản hợp lệ giống XML được xây dựng từ các thẻ có dạng <a và </a, trong đó tên thẻ là một chữ cái viết thường. Các thẻ có thể được lồng vào nhau và nhiều đoạn XML có thể xuất hiện lần lượt. Nhiệm vụ không phải là xác thực XML. Hiệu lực đã được đảm bảo."
date: "2026-06-02T16:18:01+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 125
codeforces_index: "B"
codeforces_contest_name: "Codeforces Testing Round 2"
rating: 1000
weight: 125
solve_time_s: 106
verified: true
draft: false
---

[CF 125B - XML đơn giản](https://codeforces.com/problemset/problem/125/B) 

**Đánh giá:** 1000 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 46s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Đầu vào là một văn bản giống XML hợp lệ được xây dựng từ các thẻ có dạng`<a>`Và`</a>`, trong đó tên thẻ là một chữ cái viết thường. Các thẻ có thể được lồng vào nhau và nhiều đoạn XML có thể xuất hiện lần lượt. 

Nhiệm vụ không phải là xác thực XML. Hiệu lực đã được đảm bảo. Chúng ta chỉ cần định dạng lại nó. 

Mỗi thẻ phải được in trên dòng riêng của nó. Việc thụt lề phụ thuộc vào độ sâu lồng nhau. Một thẻ ở độ sâu`h`được đặt trước bởi`2 * h`không gian. 

Ví dụ, trong```
<a><b></b></a>
```thẻ`<a>`đang ở độ sâu`0`,`<b>`ở bên trong`<a>`vậy là nó ở độ sâu`1`, Và`</b>`cũng được in ở độ sâu`1`. 

Độ dài đầu vào tối đa là 1000 ký tự. Cái này nhỏ xíu. Thậm chí một`O(n²)`giải pháp sẽ phù hợp một cách thoải mái, nhưng cấu trúc của vấn đề đương nhiên sẽ dẫn đến việc quét tuyến tính đơn giản. 

Thách thức chính là xử lý thụt lề một cách chính xác để đóng thẻ. Một lỗi phổ biến là in thẻ đóng bằng độ sâu hiện tại và chỉ sau đó giảm độ sâu. Coi như:```
<a></a>
```Đầu ra đúng:```
<a>
</a>
```Nếu chúng ta in`</a>`trước khi giảm mức độ lồng nhau, nó sẽ bị thụt vào hai khoảng trắng một cách không chính xác. 

Một trường hợp tinh vi khác là thẻ đóng ngay sau thẻ mở:```
<a><b></b></a>
```Đầu ra đúng:```
<a>
  <b>
  </b>
</a>
```Độ sâu cho`</b>`giống như độ sâu của`<b>`. Chúng ta phải giảm mức độ lồng nhau trước khi in thẻ đóng. 

Trường hợp thứ ba là nhiều phần tử anh chị em:```
<a></a><b></b>
```Đầu ra đúng:```
<a>
</a>
<b>
</b>
```Sau khi hoàn thành một phần tử, mức lồng nhau phải trở về giá trị trước đó để phần tử anh em tiếp theo bắt đầu thụt lề chính xác. 

## Phương pháp tiếp cận 

Một cách mạnh mẽ để giải quyết vấn đề là liên tục xác định vị trí thẻ tiếp theo, xác định mức độ lồng của nó bằng cách kiểm tra tất cả các thẻ đã xử lý trước đó và sau đó in nó. Cách tiếp cận như vậy tính toán lại thông tin chuyên sâu nhiều lần. Với một chuỗi dài`n`, điều đó có thể dẫn đến`O(n²)`công việc. 

Kích thước đầu vào đủ nhỏ để thậm chí điều này có thể vượt qua, nhưng có một giải pháp rõ ràng hơn nhiều. 

Quan sát quan trọng là độ sâu lồng nhau chỉ thay đổi khi chúng ta gặp thẻ. 

Thẻ mở làm tăng độ sâu cho mọi thứ bên trong nó. Thẻ đóng kết thúc vùng lồng nhau hiện tại và giảm độ sâu. 

Điều này có nghĩa là chúng ta có thể xử lý văn bản XML từ trái sang phải chính xác một lần. Bất cứ khi nào chúng tôi gặp một thẻ, chúng tôi sẽ xác định xem thẻ đó đang mở hay đóng. 

Đối với thẻ mở:```
<a>
```chúng tôi in nó bằng độ sâu hiện tại, sau đó tăng độ sâu. 

Đối với thẻ đóng:```
</a>
```đầu tiên chúng tôi giảm độ sâu, sau đó in nó. 

Quy tắc duy nhất đó tự động tạo ra thụt lề chính xác cho mọi cấu trúc XML hợp lệ. 

Vì mỗi ký tự chỉ được kiểm tra một lần trong khi trích xuất thẻ nên giải pháp sẽ chạy theo thời gian tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Được chấp nhận nhưng không cần thiết | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

các`O(n)`không gian phụ trợ đến từ việc lưu trữ các dòng đầu ra trước khi in chúng. 

## Hướng dẫn thuật toán 

1. Đọc chuỗi XML. 
2. Quét chuỗi từ trái sang phải. 
3. Bất cứ khi nào một`<`được tìm thấy, tiếp tục cho đến khi khớp`>`để trích xuất thẻ hoàn chỉnh. 
4. Kiểm tra xem thẻ có phải là thẻ đóng hay không. Thẻ đóng bắt đầu bằng`</`. 
5. Nếu thẻ là thẻ đóng, hãy giảm độ sâu hiện tại xuống một trước khi in. Phần tử bị đóng thuộc về cấp độ gốc chứ không phải cấp độ bên trong chính nó. 
6. In thẻ đứng trước`2 × depth`không gian. 
7. Nếu thẻ là thẻ mở, hãy tăng độ sâu thêm một sau khi in. Mọi thứ bên trong phần tử đó đều sâu hơn một cấp. 
8. Tiếp tục cho đến khi toàn bộ chuỗi được xử lý. 

### Tại sao nó hoạt động 

Giữ nguyên bất biến đó`depth`luôn bằng số lượng thẻ hiện đang mở có thẻ đóng chưa được xử lý. 

Trước khi thẻ mở được in, nó thuộc về cấp độ lồng hiện tại, do đó việc in với độ sâu hiện tại là chính xác. Sau khi in, thẻ đó sẽ hoạt động nên độ sâu sẽ tăng thêm một. 

Trước khi thẻ đóng được in, phần tử tương ứng không còn được coi là hoạt động. Việc giảm độ sâu trước tiên sẽ khôi phục mức độ lồng nhau của phần tử gốc. Việc in với độ sâu cập nhật này mang lại chính xác mức thụt lề theo yêu cầu của câu lệnh. 

Vì mỗi thẻ đều cập nhật độ sâu theo quy tắc lồng nhau của XML nên mỗi dòng đều được in ở cấp độ chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = input().strip()

    depth = 0
    lines = []

    i = 0
    n = len(s)

    while i < n:
        j = i
        while s[j] != '>':
            j += 1

        tag = s[i:j + 1]

        if tag.startswith("</"):
            depth -= 1
            lines.append("  " * depth + tag)
        else:
            lines.append("  " * depth + tag)
            depth += 1

        i = j + 1

    sys.stdout.write("\n".join(lines))

if __name__ == "__main__":
    solve()
```Quá trình quét sử dụng hai con trỏ.`i`đánh dấu sự bắt đầu của thẻ hiện tại và`j`tiến lên cho đến khi khớp`>`được tìm thấy. 

Thứ tự các thao tác đóng thẻ là chi tiết quan trọng nhất trong quá trình triển khai. Độ sâu phải được giảm trước khi tạo vết lõm. Đảo ngược hai thao tác đó sẽ dịch chuyển mỗi thẻ đóng sang bên phải một cấp. 

Đối với thẻ mở, thứ tự ngược lại là bắt buộc. Bản thân thẻ thuộc về cấp độ hiện tại nên nó được in trước và chỉ sau đó mới tăng độ sâu lồng ghép cho nội dung của nó. 

Đầu vào được đảm bảo chỉ chứa văn bản XML hợp lệ, do đó không cần logic xác thực hoặc ngăn xếp. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
<a><b><c></c></b></a>
```| Gắn thẻ | Đóng cửa? | Độ sâu trước | Độ sâu được sử dụng | Độ sâu sau | 
| --- | --- | --- | --- | --- | 
|`<a>`| Không | 0 | 0 | 1 | 
|`<b>`| Không | 1 | 1 | 2 | 
|`<c>`| Không | 2 | 2 | 3 | 
|`</c>`| Có | 3 | 2 | 2 | 
|`</b>`| Có | 2 | 1 | 1 | 
|`</a>`| Có | 1 | 0 | 0 | 

Đầu ra:```
<a>
  <b>
    <c>
    </c>
  </b>
</a>
```Dấu vết này cho thấy sự bất biến trung tâm. Thẻ mở in ở độ sâu hiện tại và sau đó tăng nó lên. Thẻ đóng giảm trước rồi mới in. 

### Ví dụ 2 

đầu vào:```
<a></a><b></b>
```| Gắn thẻ | Đóng cửa? | Độ sâu trước | Độ sâu được sử dụng | Độ sâu sau | 
| --- | --- | --- | --- | --- | 
|`<a>`| Không | 0 | 0 | 1 | 
|`</a>`| Có | 1 | 0 | 0 | 
|`<b>`| Không | 0 | 0 | 1 | 
|`</b>`| Có | 1 | 0 | 0 | 

Đầu ra:```
<a>
</a>
<b>
</b>
```Ví dụ này thể hiện các phần tử anh chị em. Sau khi một phần tử đóng lại, độ sâu sẽ trở về giá trị trước đó, do đó phần tử anh em tiếp theo bắt đầu thụt lề chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi nhân vật được truy cập một số lần không đổi | 
| Không gian | O(n) | Các dòng đầu ra được lưu trữ trước khi in | 

Với độ dài đầu vào tối đa chỉ 1000 ký tự, giải pháp tuyến tính về cơ bản chạy ngay lập tức và sử dụng bộ nhớ không đáng kể. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    s = inp.strip()

    depth = 0
    lines = []

    i = 0
    while i < len(s):
        j = i
        while s[j] != '>':
            j += 1

        tag = s[i:j + 1]

        if tag.startswith("</"):
            depth -= 1
            lines.append("  " * depth + tag)
        else:
            lines.append("  " * depth + tag)
            depth += 1

        i = j + 1

    return "\n".join(lines)

# provided sample
assert run("<a><b><c></c></b></a>") == (
    "<a>\n"
    "  <b>\n"
    "    <c>\n"
    "    </c>\n"
    "  </b>\n"
    "</a>"
), "sample 1"

# minimum valid non-empty XML
assert run("<a></a>") == (
    "<a>\n"
    "</a>"
), "single element"

# siblings
assert run("<a></a><b></b>") == (
    "<a>\n"
    "</a>\n"
    "<b>\n"
    "</b>"
), "siblings"

# deeper nesting
assert run("<a><b></b></a>") == (
    "<a>\n"
    "  <b>\n"
    "  </b>\n"
    "</a>"
), "nested pair"

# mixed nesting and siblings
assert run("<a><b></b><c></c></a>") == (
    "<a>\n"
    "  <b>\n"
    "  </b>\n"
    "  <c>\n"
    "  </c>\n"
    "</a>"
), "multiple children"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`<a></a>`| Hai dòng không thụt lề | XML không trống nhỏ nhất | 
|`<a></a><b></b>`| Hai yếu tố anh chị em | Đặt lại độ sâu sau khi đóng | 
|`<a><b></b></a>`| Một đứa trẻ lồng nhau | Thụt lề chính xác của thẻ đóng | 
|`<a><b></b><c></c></a>`| Hai đứa con bên trong cha mẹ | Trộn tổ và anh chị em | 

## Vỏ cạnh 

Hãy xem xét XML hợp lệ nhỏ nhất:```
<a></a>
```Xử lý`<a>`in nó ở độ sâu`0`và tăng độ sâu lên`1`. Khi`</a>`đạt được, thuật toán giảm độ sâu trở lại`0`trước khi in. Đầu ra trở thành:```
<a>
</a>
```điều đó hoàn toàn chính xác 

Xem xét việc lồng ngay lập tức:```
<a><b></b></a>
```Sau khi in`<b>`, độ sâu trở thành`2`. Khi`</b>`xuất hiện, trước tiên thuật toán giảm độ sâu xuống`1`và sau đó in. Đầu ra là:```
<a>
  <b>
  </b>
</a>
```Nếu sự giảm xảy ra sau khi in,`</b>`sẽ nhận không chính xác bốn khoảng trống. 

Hãy xem xét các yếu tố anh chị em:```
<a></a><b></b>
```Sau đó`</a>`được xử lý, độ sâu trở về`0`. Thẻ tiếp theo`<b>`được in không có vết lõm. Đầu ra là:```
<a>
</a>
<b>
</b>
```Điều này xác nhận rằng việc đóng cây con sẽ khôi phục hoàn toàn mức độ lồng của cây cha trước khi xử lý các phần tử tiếp theo.
