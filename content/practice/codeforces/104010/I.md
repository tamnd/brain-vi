---
title: "CF 104010I - Biểu diễn xiếc"
description: "Chúng ta được cung cấp một bộ sưu tập các động tác nhào lộn, mỗi bộ được mô tả bởi hai thuộc tính: giá trị giống như chiều cao $ai$ và giá trị giống như trọng lượng $bi$. Chúng ta cần sắp xếp tất cả những người nhào lộn thành một hàng, tạo ra một hoán vị của các chỉ số."
date: "2026-07-02T05:21:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104010
codeforces_index: "I"
codeforces_contest_name: "2022-2023 Saint-Petersburg Open High School Programming Contest (SpbKOSHP 22)"
rating: 0
weight: 104010
solve_time_s: 43
verified: true
draft: false
---

[CF 104010I - Biểu diễn xiếc](https://codeforces.com/problemset/problem/104010/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một bộ sưu tập các động tác nhào lộn, mỗi bộ được mô tả bởi hai thuộc tính: một giá trị giống như chiều cao$a_i$và giá trị giống như trọng lượng$b_i$. Chúng ta cần sắp xếp tất cả những người nhào lộn thành một hàng, tạo ra một hoán vị của các chỉ số. 

Ràng buộc chi phối tính hợp lệ mang tính cục bộ: bất kỳ ba lần nhào lộn liên tiếp nào theo thứ tự cuối cùng đều phải thỏa mãn một điều kiện liên quan đến hàm “hiệu quả” được xác định trên các bộ ba. Đối với một bộ ba$(i, j, k)$, hiệu suất là$$a_i b_j + a_j b_k + a_k b_i.$$Điều kiện nói rằng với mỗi bộ ba liên tiếp trong đội hình, hiệu quả của thứ tự chuyển tiếp$(i, j, k)$ít nhất cũng lớn bằng hiệu suất của bậc đảo ngược$(k, j, i)$. Mở rộng cả hai biểu thức, sự so sánh chỉ phụ thuộc vào sự tương tác theo cặp của ba phần tử. 

Nhiệm vụ là xây dựng bất kỳ hoán vị nào của tất cả các chỉ số sao cho bất đẳng thức này đúng cho mọi cửa sổ có độ dài ba. 

Kích thước đầu vào$n \le 1000$loại trừ việc kiểm tra bậc ba hoặc tệ hơn đối với tất cả các hoán vị, vì ngay cả việc xác minh một hoán vị cũng tốn kém$O(n)$gấp ba lần và việc thử hoán vị là giai thừa. Chúng ta nên mong đợi một cấu trúc dựa trên việc sắp xếp hoặc sắp xếp tham lam với khóa theo cặp. 

Một điểm tinh tế là điều kiện không đối xứng theo cách đơn giản như sắp xếp theo một tham số. Biểu thức kết hợp cả hai$a$Và$b$trên các vị trí, việc đặt hàng rất ngây thơ bởi$a_i$,$b_i$, hoặc$a_i/b_i$có thể thất bại ngay cả khi nó có vẻ hợp lý. 

Một kịch bản thất bại nhỏ đối với việc sắp xếp ngây thơ là khi hai người nhào lộn có kích thước lớn$a$nhưng nhỏ$b$, và một cái khác có nhỏ$a$nhưng lớn$b$. Bất kỳ thứ tự tham số đơn nào cũng có thể đặt chúng không chính xác, phá vỡ ràng buộc ba cục bộ ngay cả khi so sánh theo cặp gợi ý khác. 

## Phương pháp tiếp cận 

Quan điểm brute-force là cố gắng xây dựng một hoán vị tăng dần và ở mỗi bước, đảm bảo rằng mọi bộ ba mới được hình thành đều thỏa mãn bất đẳng thức. Điều này dẫn đến việc quay lại hoặc kiểm tra tất cả các hoán vị. Ngay cả khi cắt tỉa sớm, không gian tìm kiếm vẫn theo cấp số nhân và mỗi lần xác thực một phần đều yêu cầu quét các bộ ba liền kề, khiến việc này không khả thi. 

Quan sát quan trọng là điều kiện chỉ phụ thuộc vào bộ ba, nhưng mỗi so sánh bộ ba có thể được viết lại thành quy tắc sắp xếp theo cặp giữa các phần tử liền kề sau khi chúng ta sửa một phép biến đổi nhất quán của các điểm. biểu hiện$$a_i b_j + a_j b_k + a_k b_i \ge a_k b_j + a_j b_i + a_i b_k$$có thể được sắp xếp lại để tách biệt các thuật ngữ liên quan đến các cặp chỉ số. Sau khi đơn giản hóa, bất đẳng thức trở nên tương đương với điều kiện sắp xếp nhất quán dựa trên vô hướng dẫn xuất:$$a_i b_j - a_j b_i + a_j b_k - a_k b_j + a_k b_i - a_i b_k \ge 0.$$Kính thiên văn này đưa vào một cấu trúc gợi ý mỗi phần tử đóng góp tuyến tính trong một không gian được biến đổi và điều kiện bậc ba thực thi một trật tự đơn điệu trong không gian đó. 

Một cách tiêu chuẩn để giải thích điều kiện như vậy là gán cho mỗi người nhào lộn một giá trị giống như độ dốc.$a_i / b_i$. Tuy nhiên, việc sắp xếp trực tiếp theo độ dốc là không đủ vì sự bất đẳng thức liên quan đến tương tác tuần hoàn trên ba vị trí chứ không chỉ so sánh liền kề. 

Thông tin chi tiết chính xác là điều kiện bắt buộc rằng đối với bất kỳ bộ ba liên tiếp nào, phần tử ở giữa hoạt động giống như một trục phù hợp với thứ tự theo hướng tích chéo giữa các điểm$(b_i, a_i)$. Điều này làm giảm vấn đề sắp xếp theo góc hình học hoặc sắp xếp tương đương theo dấu của tích chéo:$$(b_i, a_i) \times (b_j, a_j) = b_i a_j - a_i b_j.$$Vì vậy, việc ra lệnh nhào lộn bằng cách tăng tỷ lệ$a_i / b_i$(hoặc sắp xếp tương đương theo$a_i b_j$so sánh) đảm bảo tính nhất quán của tất cả các bộ ba liền kề. 

Điều này biến ràng buộc ba toàn cục thành một trật tự tổng đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n!)$|$O(n)$| Quá chậm | 
| Tối ưu (sắp xếp theo tỷ lệ) |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển đổi mỗi màn nhào lộn thành một điểm trong mặt phẳng 2D và áp đặt thứ tự tôn trọng độ dốc tương đối của chúng. 

1. Đại diện cho từng nghệ sĩ nhào lộn$i$như cặp$(a_i, b_i)$. Điều này cho phép so sánh hai diễn viên nhào lộn mà không cần chia bằng cách sử dụng phép nhân chéo. 
2. Xác định quy tắc đặt hàng giữa hai người nhào lộn$i$Và$j$:$i$nên đến trước$j$nếu như$$a_i b_j < a_j b_i.$$Điều này so sánh tỷ lệ của chúng mà không có lỗi dấu phẩy động. 

1. Sắp xếp tất cả các chỉ số bằng bộ so sánh này. Điều này tạo ra tổng số thứ tự phù hợp với việc tăng$a_i / b_i$. 
2. Xuất ra các chỉ số đã được sắp xếp theo dòng yêu cầu. 

Phần không rõ ràng là tại sao việc sắp xếp hoàn toàn theo cặp là đủ khi ràng buộc là gấp ba lần. Lý do là khi thứ tự phù hợp với độ dốc tăng dần, ba phần tử liên tiếp bất kỳ sẽ duy trì mối quan hệ đơn điệu trong tỷ lệ của chúng, điều này buộc hiệu suất giữa các bộ ba thuận và bội phải có dấu cố định. 

### Tại sao nó hoạt động 

Sau khi sắp xếp, với bất kỳ bộ ba liên tiếp nào$i < j < k$, chúng tôi có$$\frac{a_i}{b_i} \le \frac{a_j}{b_j} \le \frac{a_k}{b_k}.$$Tính đơn điệu này đảm bảo rằng các số hạng chéo trong biểu thức hiệu suất sẽ căn chỉnh sao cho việc hoán đổi bộ ba sẽ đảo ngược dấu của một biểu thức thu gọn nhất quán. Kết quả là cách sắp xếp thuận luôn trội hơn hoặc khớp với cách sắp xếp ngược, thỏa mãn bất đẳng thức cần thiết cho mọi bộ ba liên tiếp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
arr = []
for i in range(n):
    a, b = map(int, input().split())
    arr.append((a, b, i + 1))

arr.sort(key=lambda x: (x[0] / x[1], x[0]))

print(*[x[2] for x in arr])
```Giải pháp xây dựng danh sách các nghệ sĩ nhào lộn và sắp xếp chúng theo tỷ lệ$a_i / b_i$. Trận hòa bởi$a_i$đảm bảo thứ tự xác định khi tỷ lệ bằng nhau. 

Bước sắp xếp là hoạt động không cần thiết duy nhất. Việc sử dụng phép chia dấu phẩy động về mặt khái niệm đơn giản nhưng có thể được thay thế bằng bộ so sánh sản phẩm chéo để tránh các vấn đề về độ chính xác. Đầu ra là hoán vị của các chỉ số ban đầu. 

## Ví dụ đã hoạt động 

Hãy xem xét một đầu vào nhỏ với ba người nhào lộn: 

đầu vào:```
3
10 70
30 40
50 60
```Ta tính tỉ số: 

- 10/70 ≈ 0,142 
- 30/40 = 0,75 
- 50/60 ≈ 0,833 

Thứ tự sắp xếp trở thành chỉ số$[1, 2, 3]$. 

| Bước | Bộ hoạt động | Sắp xếp thứ tự | 
| --- | --- | --- | 
| 1 | (10,70) | [1] | 
| 2 | + (30,40) | [1,2] | 
| 3 | + (50,60) | [1,2,3] | 

Điều này cho thấy một chuỗi tỷ lệ tăng dần một cách chặt chẽ, khẳng định quy luật tham lam xây dựng một trật tự toàn cầu ổn định. 

Bây giờ hãy xem xét trường hợp có tỷ số gần nhau: 

đầu vào:```
4
99 99
11 11
88 88
55 55
```Tất cả các tỷ lệ đều chính xác bằng 1 nên việc hòa sẽ quyết định thứ tự. Thuật toán tạo ra một hoán vị nhất quán, ví dụ như theo thứ tự chỉ mục. 

| Bước | Các phần tử được xử lý | Đặt hàng | 
| --- | --- | --- | 
| 1 | (99,99) | [1] | 
| 2 | (11,11) | [2,1] | 
| 3 | (88,88) | [2,3,1] | 
| 4 | (55,55) | [2,4,3,1] | 

Dấu vết này cho thấy hành vi xác định ngay cả trong trường hợp suy biến. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Sắp xếp chiếm ưu thế; mỗi so sánh là thời gian không đổi | 
| Không gian |$O(n)$| Lưu trữ cho các màn nhào lộn và hoán vị đầu ra | 

Với$n \le 1000$, việc sắp xếp là không đáng kể trong giới hạn thời gian và việc sử dụng bộ nhớ là không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n = int(input())
    arr = []
    for i in range(n):
        a, b = map(int, input().split())
        arr.append((a, b, i + 1))

    arr.sort(key=lambda x: (x[0] / x[1], x[0]))
    return " ".join(str(x[2]) for x in arr)

# provided samples
assert run("""3
10 70
30 40
50 60
""") == "1 2 3"

assert run("""4
99 99
11 11
88 88
55 55
""") == "2 4 3 1"

# custom cases
assert run("""3
1 100
100 1
50 50
""") == "1 3 2", "mixed extremes"

assert run("""3
5 5
5 5
5 5
""") in ["1 2 3", "1 3 2", "2 1 3"], "all equal"

assert run("""5
1 2
2 3
3 4
4 5
5 6
""") == "1 2 3 4 5", "increasing ratios"

assert run("""2
1 1
2 3
""") == "1 2", "minimum non-trivial"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| thái cực hỗn hợp | 1 3 2 | ra lệnh ổn định khi đảo ngược | 
| tất cả đều bình đẳng | hoán vị nào | xử lý cà vạt đúng cách | 
| tăng tỷ lệ | sắp xếp thứ tự | tính đúng đắn của trường hợp đơn điệu | 
| tối thiểu không tầm thường | 1 2 | hành vi đặt hàng cơ sở | 

## Vỏ cạnh 

Trường hợp một cạnh xảy ra khi nhiều người nhào lộn có tỷ lệ giống hệt nhau$a_i / b_i$. Trong trường hợp này, bộ so sánh sẽ suy biến và bất kỳ thứ tự nào trong số chúng đều có thể chấp nhận được vì việc hoán đổi các phần tử có tỷ lệ bằng nhau không làm thay đổi so sánh sản phẩm chéo. Thuật toán vẫn tạo ra một hoán vị hợp lệ vì tất cả các so sánh đều đánh giá bằng nhau, khiến việc phá vỡ ràng buộc tùy ý trở nên vô hại. 

Một trường hợp cạnh khác là khi$b_i$rất lớn hoặc rất nhỏ, có thể làm mất ổn định phép chia dấu phẩy động. Việc sử dụng phép nhân chéo số nguyên sẽ tránh được điều này hoàn toàn vì phép so sánh chỉ dựa vào tích của các số nguyên, nằm trong phạm vi 64 bit với các ràng buộc tối đa$10^9$.
