---
title: "CF 103736B - Chuỗi mới"
description: "Chúng ta được cung cấp một thứ tự tùy chỉnh của bảng chữ cái viết thường tiếng Anh, trong đó 26 chữ cái xuất hiện theo một trình tự cụ thể xác định một thứ tự tổng thể nghiêm ngặt. Nếu một chữ cái xuất hiện sớm hơn theo thứ tự này thì nó được coi là nhỏ hơn."
date: "2026-07-02T09:09:52+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103736
codeforces_index: "B"
codeforces_contest_name: "The 2022 Hangzhou Normal U Summer Trials"
rating: 0
weight: 103736
solve_time_s: 49
verified: true
draft: false
---

[CF 103736B - Chuỗi mới](https://codeforces.com/problemset/problem/103736/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một thứ tự tùy chỉnh của bảng chữ cái viết thường tiếng Anh, trong đó 26 chữ cái xuất hiện theo một trình tự cụ thể xác định một thứ tự tổng thể nghiêm ngặt. Nếu một chữ cái xuất hiện sớm hơn theo thứ tự này thì nó được coi là nhỏ hơn. Điều này tạo ra một trật tự từ điển trên các chuỗi được xây dựng từ những chữ cái này. 

Sau khi xác định thứ tự bảng chữ cái tùy chỉnh này, chúng ta được cấp một tập hợp các chuỗi. Nhiệm vụ là xác định chuỗi nào sẽ xuất hiện ở vị trí K nếu chúng ta sắp xếp tất cả các chuỗi đã cho bằng cách sử dụng thứ tự từ điển tùy chỉnh này, sau đó so sánh từ điển tiêu chuẩn được mở rộng với độ dài làm điểm ngắt kết nối cuối cùng. 

Điểm mấu chốt là “nhỏ nhất về mặt từ điển” không dựa trên thông thường`'a' < 'b' < ... < 'z'`, mà thay vào đó là hoán vị được cung cấp của bảng chữ cái. 

Từ góc độ tính toán, n nhiều nhất là 1000 và độ dài mỗi chuỗi nhiều nhất là 1000. Điều này làm cho việc sắp xếp O(n log n) theo các so sánh chuỗi là khả thi, vì mỗi so sánh ở mức tệ nhất là O(1000). Việc liệt kê tất cả các hoán vị hoặc xây dựng cấu trúc xếp hạng trie đầy đủ sẽ là quá mức cần thiết. 

Một trường hợp phức tạp phát sinh từ thực tế là thứ tự bảng chữ cái không chuẩn. Ví dụ: nếu đơn hàng bắt đầu bằng`zabcdefghijklmnopqrstuvwxy`, sau đó`'z'`là nhỏ nhất và`'a'`là nhỏ thứ hai. Việc triển khai ngây thơ mà quên ánh xạ lại các ký tự sẽ tạo ra thứ tự hoàn toàn không chính xác trong khi vẫn vượt qua nhiều bài kiểm tra tình cờ. 

Một cạm bẫy tiềm tàng khác là hiểu sai các quy tắc so sánh từ điển. Việc so sánh có tính nhạy cảm với tiền tố: nếu một chuỗi là tiền tố của một chuỗi khác thì chuỗi ngắn hơn sẽ nhỏ hơn. Ví dụ, nếu`"a"`Và`"ab"`được so sánh,`"a"`phải đến trước bất kể thứ tự bảng chữ cái. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ là xác định một bộ so sánh so sánh trực tiếp hai chuỗi ký tự theo ký tự bằng cách sử dụng thứ tự bảng chữ cái đã cho, sau đó sắp xếp toàn bộ mảng. Điều này đúng vì thứ tự từ điển được xác định hoàn toàn bằng cách so sánh từng cặp. Tuy nhiên, cách tiếp cận này vẫn phụ thuộc vào việc so sánh lặp lại các chuỗi và mỗi so sánh có thể quét tối đa O(L) ký tự trong đó L là độ dài chuỗi tối đa. Với n lên tới 1000 và L lên tới 1000, việc sắp xếp mang lại khoảng O(n log n · L), tức là kiểm tra khoảng 10^7 ký tự trong trường hợp xấu nhất, vẫn được chấp nhận trong Python. 

Một quan sát có cấu trúc hơn là khó khăn duy nhất nằm ở việc so sánh các ký tự một cách hiệu quả. Sau khi chúng tôi chuyển đổi từng ký tự thành thứ hạng của nó trong bảng chữ cái tùy chỉnh, mỗi chuỗi sẽ trở thành một chuỗi số nguyên và thứ tự từ điển tiêu chuẩn sẽ được áp dụng trực tiếp. Điều này loại bỏ chi phí phải tra cứu từ điển lặp đi lặp lại bên trong so sánh và làm cho việc sắp xếp trở nên rõ ràng và nhanh hơn trong thực tế. 

Do đó, giải pháp tối ưu đơn giản là xây dựng ánh xạ từ ký tự đến cấp bậc và sắp xếp bằng cách sử dụng các phím được chuyển đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Bộ so sánh lực lượng vũ phu | O(n log n · L) | O(1) thêm | Đã chấp nhận | 
| Ánh xạ xếp hạng + Sắp xếp khóa | O(n · L log n) | O(26 + nL) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi thứ tự bảng chữ cái tùy chỉnh và xây dựng một mảng`rank`như vậy`rank[c]`đưa ra vị trí của nhân vật`c`theo thứ tự này. Điều này cho phép so sánh O(1) giữa hai ký tự bất kỳ. Lý do điều này là cần thiết vì việc so sánh chuỗi trực tiếp phụ thuộc vào việc tra cứu lặp đi lặp lại theo thứ tự không chuẩn. 
2. Đọc tất cả các chuỗi đầu vào vào một danh sách. Mỗi chuỗi sau đó sẽ được chuyển đổi thành một biểu diễn có thể so sánh được theo thứ tự tùy chỉnh. 
3. Đối với mỗi chuỗi, hãy xây dựng một phiên bản được chuyển đổi trong đó mỗi ký tự được thay thế bằng giá trị thứ hạng của nó. Điều này biến đổi so sánh từ điển theo bảng chữ cái tùy chỉnh thành so sánh từ điển thông thường trên các số nguyên. Bước này về mặt khái niệm là bình thường hóa vấn đề. 
4. Sắp xếp danh sách các chuỗi bằng cách sử dụng các biểu diễn được chuyển đổi này làm khóa sắp xếp. Sau đó, tính năng sắp xếp tích hợp của Python sẽ áp dụng chính xác thứ tự từ điển trên các bộ dữ liệu hoặc danh sách số nguyên. 
5. Xuất chuỗi thứ K từ danh sách ban đầu sau khi sắp xếp. Chúng tôi giữ các chuỗi gốc cùng với các khóa đã chuyển đổi của chúng để không làm mất định dạng đầu ra thực tế. 

### Tại sao nó hoạt động 

Phép biến đổi duy trì thứ tự vì quy tắc so sánh từ điển chỉ phụ thuộc vào thứ tự tương đối của các ký tự chứ không phụ thuộc vào danh tính thực tế của chúng. Bằng cách ánh xạ từng ký tự theo thứ hạng của nó trong bảng chữ cái tùy chỉnh, chúng tôi nhúng thứ tự tùy chỉnh vào thứ tự số nguyên tiêu chuẩn. Vì Python so sánh các chuỗi theo từ điển nên việc so sánh các mảng được chuyển đổi tương đương với việc so sánh các chuỗi gốc theo bảng chữ cái đã cho. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    order = input().strip()
    rank = [0] * 26
    for i, ch in enumerate(order):
        rank[ord(ch) - 97] = i

    n = int(input())
    arr = []
    for _ in range(n):
        s = input().strip()
        key = tuple(rank[ord(c) - 97] for c in s)
        arr.append((key, s))

    k = int(input())
    arr.sort(key=lambda x: x[0])
    print(arr[k - 1][1])

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ xây dựng ánh xạ trực tiếp từ các ký tự đến các chỉ mục thứ tự tùy chỉnh của chúng. Điều này tránh việc quét lặp lại chuỗi bảng chữ cái trong quá trình so sánh. 

Mỗi chuỗi được ghép nối với một bộ biểu diễn các ký tự của nó theo thứ tự mới. Một bộ dữ liệu được sử dụng vì Python so sánh các bộ dữ liệu theo từ điển theo O (độ dài của bộ dữ liệu), điều này căn chỉnh chính xác với hành vi so sánh chuỗi dự kiến. 

Việc sắp xếp được thực hiện bằng cách sử dụng các phím này và chúng tôi trích xuất phần tử thứ K sau khi sắp xếp. 

Một lỗi tinh vi phổ biến là quên chuyển đổi các ký tự thành cấp bậc và thay vào đó dựa vào thứ tự ASCII, điều này sẽ làm mất hiệu lực hoàn toàn kết quả bất cứ khi nào bảng chữ cái tùy chỉnh khác với thứ tự tiêu chuẩn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
acbdefghijklmnopqrstuvwxyz
abc
acb
2
```Phương tiện đặt hàng tùy chỉnh`'a' < 'c' < 'b' < ...`. 

| Chuỗi | Khóa chuyển đổi | 
| --- | --- | 
| abc | (0, 2, 1) | 
| acb | (0, 1, 2) | 

Thứ tự sắp xếp trở thành:`acb`,`abc`Với K = 2, đầu ra là`abc`. 

Điều này chứng tỏ việc hoán đổi chỉ hai ký tự trong bảng chữ cái sẽ đảo ngược hoàn toàn sự so sánh giữa các chuỗi có chung các ký tự đó. 

### Ví dụ 2 

đầu vào:```
zabcdefghijklmnopqrstuvwxy
a
b
1
```Đây`'z'`thì nhỏ nhất`'a'`, sau đó`'b'`. 

| Chuỗi | Chìa khóa | 
| --- | --- | 
| một | (1,) | 
| b | (2,) | 

Thứ tự sắp xếp là`a`,`b`. Với K = 1, đầu ra là`a`. 

Điều này xác nhận việc so sánh một ký tự không có tiền tố trong bảng chữ cái không chuẩn hoạt động giống hệt như so sánh số nguyên sau khi ánh xạ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n · L) | Sắp xếp n chuỗi có độ dài L bằng cách sử dụng so sánh bộ từ điển | 
| Không gian | O(nL) | Lưu trữ các khóa được chuyển đổi cho tất cả các chuỗi | 

Cho n ≤ 1000 và L ≤ 1000, điều này phù hợp một cách thoải mái trong giới hạn. Yếu tố chi phối là sắp xếp, nhưng các hằng số nhỏ và Python xử lý việc so sánh bộ dữ liệu một cách hiệu quả. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# sample-like case
assert run("""acbdefghijklmnopqrstuvwxyz
3
abc
acb
bca
2
""") == "abc"

# already sorted case
assert run("""abcdefghijklmnopqrstuvwxyz
3
a
aa
b
1
""") == "a"

# reverse alphabet
assert run("""zyxwvutsrqponmlkjihgfedcba
3
a
b
c
3
""") == "a"

# identical prefixes
assert run("""abcdefghijklmnopqrstuvwxyz
4
abc
ab
abcd
a
3
""") == "abc"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| thứ tự bảng chữ cái hỗn hợp | abc | tùy chỉnh đặt hàng chính xác | 
| bảng chữ cái tự nhiên | một | dự phòng từ điển chuẩn | 
| bảng chữ cái đảo ngược | một | xử lý đảo ngược hoàn toàn | 
| tập nặng tiền tố | abc | quy tắc so sánh tiền tố | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi các chuỗi giống hệt nhau ngoại trừ các ký tự có thứ tự không tầm thường trong bảng chữ cái tùy chỉnh. Ví dụ, với việc đặt hàng`acb...`, so sánh`"ab"`Và`"ac"`phụ thuộc hoàn toàn vào thứ tự tương đối của`b`Và`c`. Thuật toán xử lý việc này một cách chính xác vì cả hai ký tự đều được chuyển đổi thành thứ hạng trước khi so sánh. 

Một trường hợp khác là mối quan hệ tiền tố. Coi như`"ab"`Và`"a"`. Kể cả nếu`b`nhỏ hơn hầu hết các ký tự trong bảng chữ cái,`"a"`vẫn phải đến trước vì phép so sánh kết thúc ở cuối chuỗi ngắn hơn. Biểu diễn tuple tự động bảo toàn điều này vì`(rank('a'),)`ngắn hơn`(rank('a'), rank('b'))`và so sánh bộ dữ liệu từ điển của Python coi tiền tố ngắn hơn là nhỏ hơn. 

Cuối cùng, các trường hợp có chuỗi có độ dài tối đa được xử lý an toàn vì phép chuyển đổi là tuyến tính trên mỗi chuỗi và không sửa đổi cấu trúc ngoài chuyển đổi, đảm bảo không xuất hiện đệ quy ẩn hoặc chi phí sao chép sâu.
