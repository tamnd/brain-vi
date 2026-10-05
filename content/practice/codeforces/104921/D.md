---
title: "CF 104921D - Thảm quà tặng"
description: "Tấm thảm là một mạng lưới nhỏ gồm các chữ cái viết thường. Bạn đọc từng cột từ trái sang phải, nhưng bạn không bị buộc phải lấy từng ký tự. Từ mỗi cột, bạn có thể chọn chính xác một chữ cái từ cột đó hoặc bỏ qua toàn bộ cột."
date: "2026-06-28T18:07:15+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104921
codeforces_index: "D"
codeforces_contest_name: "Easy_Training"
rating: 0
weight: 104921
solve_time_s: 82
verified: false
draft: false
---

[CF 104921D - Thảm quà tặng](https://codeforces.com/problemset/problem/104921/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 22s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Tấm thảm là một mạng lưới nhỏ gồm các chữ cái viết thường. Bạn đọc từng cột từ trái sang phải, nhưng bạn không bị buộc phải lấy từng ký tự. Từ mỗi cột, bạn có thể chọn chính xác một chữ cái từ cột đó hoặc bỏ qua toàn bộ cột. Mục đích là để xem liệu bạn có thể tạo từ “vika” theo thứ tự này hay không, bằng cách chọn bốn cột riêng biệt, tăng dần chỉ số từ trái sang phải, trong đó cột được chọn đầu tiên đóng góp chữ 'v', cột thứ hai là 'i', cột thứ ba là 'k' và cột thứ tư là 'a'. 

Điều quan trọng không phải là cấu trúc đầy đủ của lưới mà chỉ là liệu mỗi cột có chứa ít nhất một lần xuất hiện của chữ cái được yêu cầu hay không. Mỗi cột hoạt động giống như một “bộ sẵn sàng có/không có” cho các chữ cái. Khi một cột được sử dụng cho một ký tự, nó không thể được sử dụng lại cho ký tự khác vì các cột phải khác biệt và có thứ tự. 

Các ràng buộc là nhỏ, với cả hai chiều nhiều nhất là 20. Điều này ngay lập tức loại trừ mọi nhu cầu tối ưu hóa nhiều. Ngay cả việc kiểm tra mọi lựa chọn cột có thể cũng khả thi vì tổng số cột nhiều nhất là 20 và sự kết hợp của bốn cột sẽ bị giới hạn. 

Một trường hợp lỗi tinh tế xuất hiện khi một cột chứa nhiều chữ cái liên quan. Ví dụ: một cột có thể chứa cả 'v' và 'i'. Nó vẫn có thể sử dụng được nhưng chỉ trong một bước duy nhất trong trình tự. Một trường hợp góc khác là khi nhiều cột chứa cùng một chữ cái; chỉ có vấn đề về thứ tự chứ không phải tính duy nhất của các chữ cái trên các cột. 

Một sai lầm ngây thơ là cố gắng chọn các chữ cái theo hàng hoặc coi lưới như một vấn đề về đường dẫn chung. Ví dụ: trong một lưới như:```
v a
i k
```Ai đó có thể giả định không chính xác việc truyền tải dựa trên đường dẫn là cần thiết, nhưng vấn đề hoàn toàn bỏ qua chuyển động của hàng. Chỉ có thành viên cột quan trọng. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là thử mọi cách để chọn bốn cột riêng biệt theo thứ tự tăng dần và kiểm tra xem chúng có khớp với dãy v, i, k, a hay không. Đối với mỗi bốn cột, chúng tôi quét các hàng bên trong mỗi cột để xem ký tự được yêu cầu có tồn tại hay không. 

Điều này hiệu quả vì lưới rất nhỏ nhưng số lượng bộ tứ vẫn có thể lớn trong trường hợp xấu nhất. Với tối đa 20 cột, số cách để chọn 4 là 4845 và với mỗi cách chúng tôi có thể quét tối đa 20 hàng trên mỗi cột, dẫn đến khoảng 4845 × 80 lượt kiểm tra cho mỗi trường hợp kiểm thử. Trên 100 trường hợp thử nghiệm, điều này vẫn có thể chấp nhận được, nhưng nó là chi phí không cần thiết. 

Quan sát quan trọng là mỗi cột có thể được nén thành một trạng thái đơn giản: liệu nó có chứa 'v', 'i', 'k' hay 'a' hay không. Khi điều này được thực hiện, vấn đề sẽ tương đương với việc kiểm tra xem dãy v → i → k → a có xuất hiện dưới dạng dãy con trong danh sách các cột hay không. Điều này biến nhiệm vụ thành một lần quét tuyến tính trong đó chúng tôi nhanh chóng di chuyển qua các cột và khớp với ký tự được yêu cầu tiếp theo bất cứ khi nào có thể. 

Chúng ta không còn cần phải xem xét các kết hợp một cách rõ ràng vì mọi lựa chọn hợp lệ đều phải tôn trọng thứ tự cột và tiến trình tham lam từ trái sang phải sẽ bảo toàn tất cả các khả năng hợp lệ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên cột tăng gấp bốn lần | O(t · m⁴ · n) | O(1) | Được chấp nhận nhưng không cần thiết | 
| Kiểm tra trình tự tham lam | O(t · n · m) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi cột, hãy xác định xem nó có chứa các ký tự 'v', 'i', 'k' hay 'a' hay không. Điều này làm giảm mỗi cột thành một tập hợp cờ đơn giản thay vì danh sách đầy đủ các chữ cái. Cấu trúc lưới không còn cần thiết sau lần nén này. 
2. Tạo chuỗi mục tiêu ['v', 'i', 'k', 'a']. Chúng tôi sẽ cố gắng khớp những thứ này theo thứ tự bằng cách sử dụng các cột từ trái sang phải. 
3. Duy trì con trỏ p bắt đầu từ 0, biểu thị ký tự tiếp theo trong chuỗi mục tiêu mà chúng ta vẫn cần khớp. 
4. Lặp qua các cột từ trái sang phải. Đối với mỗi cột, hãy kiểm tra xem nó có chứa ký tự target[p] hay không. Nếu có, hãy tiến lên p từng cái một. Điều này mô phỏng việc chọn cột này cho ký tự được yêu cầu tiếp theo. 
5. Dừng sớm nếu p đạt 4, nghĩa là tất cả các ký tự đã được ghép thành công. Tại thời điểm đó chúng tôi đã biết câu trả lời là tích cực. 
6. Sau khi quét tất cả các cột, kiểm tra xem p có bằng 4 hay không. Nếu có, xuất “YES”, nếu không thì xuất “NO”. 

### Tại sao nó hoạt động 

Bất kỳ giải pháp hợp lệ nào đều tương ứng với việc chọn bốn chỉ số cột tăng dần, mỗi chỉ số đáp ứng một yêu cầu ký tự cụ thể. Nếu một chuỗi như vậy tồn tại, việc quét từ trái sang phải và tham lam tiêu thụ cột hợp lệ sớm nhất có thể cho mỗi ký tự sẽ không bao giờ chặn các kết quả trùng khớp trong tương lai. Điều này là do việc trì hoãn một trận đấu không thể tạo ra những cơ hội mới chưa có trước đó; các cột chỉ là các ràng buộc theo thứ tự chứ không phải tài nguyên có giới hạn dung lượng. Do đó, việc kiểm tra dãy con tham lam sẽ bảo tồn sự tồn tại của bất kỳ bộ bốn hợp lệ nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        cols = [set() for _ in range(m)]

        for i in range(n):
            row = input().strip()
            for j, ch in enumerate(row):
                cols[j].add(ch)

        target = "vika"
        p = 0

        for j in range(m):
            if p < 4 and target[p] in cols[j]:
                p += 1

        print("YES" if p == 4 else "NO")

if __name__ == "__main__":
    solve()
```Giải pháp trước tiên nén mỗi cột thành một tập hợp ký tự, cho phép kiểm tra tư cách thành viên O(1) đối với các chữ cái được yêu cầu. Điều này tránh việc quét liên tục các hàng sau này. 

Sau đó, vòng lặp chính thực hiện một lần chuyển từ trái sang phải qua các cột, đưa con trỏ qua chuỗi “vika”. Điều kiện thoát sớm là ẩn: khi con trỏ đạt tới 4, tất cả các chữ cái bắt buộc đã được tìm thấy theo thứ tự cột tăng dần. 

Một lỗi triển khai phổ biến là đặt lại con trỏ cho mỗi cột hoặc cố gắng khớp tất cả các chữ cái trong một cột. Điều đó sẽ cho phép sử dụng lại một cột cho nhiều ký tự một cách không chính xác, vi phạm yêu cầu về cột riêng biệt. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 4, m = 4
v i k a
v i k a
v i k a
v i k a
```Cột nêu: 

| Cột | Thư | Trận đấu | 
| --- | --- | --- | 
| 0 | v | v | 
| 1 | tôi | tôi | 
| 2 | k | k | 
| 3 | một | một | 

| Bước | Cột | Mục tiêu cần thiết | Cuộc thi đấu? | Con trỏ p | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | v | vâng | 1 | 
| 2 | 1 | tôi | vâng | 2 | 
| 3 | 2 | k | vâng | 3 | 
| 4 | 3 | một | vâng | 4 | 

Con trỏ đạt chính xác 4 sau khi xử lý cột thứ tư, xác nhận rằng từ có thể được hình thành theo thứ tự. 

### Ví dụ 2 

đầu vào:```
n = 2, m = 3
v a c
i x z
```Cột nêu: 

| Cột | Thư | 
| --- | --- | 
| 0 | v, tôi | 
| 1 | một, x | 
| 2 | c, z | 

| Bước | Cột | Mục tiêu cần thiết | Cuộc thi đấu? | Con trỏ p | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | v | vâng | 1 | 
| 2 | 1 | tôi | không | 1 | 
| 3 | 2 | tôi | không | 1 | 

Chúng ta không bao giờ đạt đến ‘i’, vì vậy quá trình kết thúc với p = 1, tạo ra “KHÔNG”. 

Điều này cho thấy rằng ngay cả khi một cột chứa nhiều chữ cái hữu ích thì chỉ có thể sử dụng một chữ cái và các ràng buộc về thứ tự sẽ ngăn việc bỏ qua ngược lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t · n · m) | Mỗi ô được đọc một lần để xây dựng các tập cột, sau đó mỗi cột được quét một lần cho mỗi trường hợp kiểm thử | 
| Không gian | O(m) | Mỗi cột lưu trữ tối đa n ký tự trong một tập hợp | 

Các giới hạn n, m ≤ 20 làm cho thời gian này không đổi một cách hiệu quả trên mỗi trường hợp thử nghiệm. Ngay cả với 100 trường hợp thử nghiệm, giải pháp vẫn chạy dưới giới hạn rất nhiều. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output

    import sys as _sys
    input = _sys.stdin.readline

    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        cols = [set() for _ in range(m)]

        for i in range(n):
            row = input().strip()
            for j, ch in enumerate(row):
                cols[j].add(ch)

        target = "vika"
        p = 0
        for j in range(m):
            if p < 4 and target[p] in cols[j]:
                p += 1

        print("YES" if p == 4 else "NO")

    sys.stdout.seek(0)
    return sys.stdout.read().strip()

# provided samples
assert run("""5
1 4
vika
3 3
bad
car
pet
4 4
vvvv
iiii
kkkk
aaaa
4 4
vkak
iiai
avvk
viaa
4 7
vbickda
vbickda
vbickda
vbickda
""") == """YES
NO
YES
NO
YES"""

# custom cases
assert run("""1
1 4
viii
""") == "NO", "missing characters"

assert run("""1
4 1
v
i
k
a
""") == "NO", "only one column"

assert run("""1
2 5
vxxxx
ixxka
""") == "YES", "spread across columns"

assert run("""1
3 4
abcd
efgh
ijkl
""") == "NO", "no relevant letters"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| ký tự bị thiếu | KHÔNG | trình tự không đầy đủ | 
| chỉ một cột | KHÔNG | yêu cầu cột riêng biệt | 
| trải rộng khắp các cột | CÓ | tích lũy tham lam có tác dụng | 
| không có chữ cái liên quan | KHÔNG | trường hợp phù hợp trống | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi nhiều chữ cái bắt buộc xuất hiện trong cùng một cột. Ví dụ:```
v i k a
v v v v
v i k a
```Cột 1 chứa cả 'v' và 'i'. Thuật toán chỉ xử lý nó một lần cho ký tự cần thiết đầu tiên. Nó sử dụng 'v' và tiến tới 'i' sau đó khi một cột khác cung cấp nó. Điều này duy trì tính chính xác vì một cột không thể đáp ứng nhiều hơn một vị trí trong chuỗi. 

Một trường hợp khác là khi các chữ cái xuất hiện theo thứ tự ngược lại trên các cột:```
a k i v
a k i v
```Mặc dù tất cả các chữ cái bắt buộc đều tồn tại ở đâu đó, các khối yêu cầu từ trái sang phải tạo thành “vika”. Quá trình quét tham lam không bao giờ tìm thấy 'v' đủ sớm, do đó con trỏ vẫn ở mức 0 và đầu ra trở thành "KHÔNG". 

Trường hợp cạnh cuối cùng là đầu vào tối thiểu:```
1 1
v
```Chỉ tồn tại một cột nên không thể chọn bốn cột riêng biệt. Quá trình quét tiêu tốn tối đa một ký tự và trả về chính xác “KHÔNG” mà không cần bất kỳ xử lý đặc biệt nào.
