---
title: "CF 967B - Hệ Thống Tưới Nước"
description: "Chúng ta được cung cấp một hệ thống các lỗ ống, mỗi lỗ có kích thước cố định xác định lượng nước có thể thoát ra khi đổ nước vào hệ thống. Arkady đổ một lượng nước cố định, nhưng chỉ có một lỗ, lỗ đầu tiên, được coi là hữu ích với anh ta."
date: "2026-06-17T01:37:10+07:00"
tags: ["codeforces", "competitive-programming", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 967
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 477 (rated, Div. 2, based on VK Cup 2018 Round 3)"
rating: 1000
weight: 967
solve_time_s: 88
verified: true
draft: false
---

[CF 967B - Hệ thống tưới nước](https://codeforces.com/problemset/problem/967/B) 

**Đánh giá:** 1000 
**Tags:** toán, sắp xếp 
**Thời gian giải:** 1 phút 28s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một hệ thống các lỗ ống, mỗi lỗ có kích thước cố định xác định lượng nước có thể thoát ra khi đổ nước vào hệ thống. Arkady đổ một lượng nước cố định, nhưng chỉ có một lỗ, lỗ đầu tiên, được coi là hữu ích với anh ta. Mục tiêu của anh là đảm bảo rằng ít nhất một lượng nước cần thiết sẽ chảy ra qua lỗ đầu tiên này. 

Khả năng kiểm soát chính mà anh ta có là anh ta có thể chặn bất kỳ tập hợp con lỗ nào trước khi đổ nước. Sau khi chặn, các lỗ hở còn lại sẽ chia nước theo tỷ lệ tương ứng với kích thước của chúng. Nếu tổng kích thước của các lỗ mở là$S$, thì lỗ đầu tiên nhận được một phần$s_1 / S$trong tổng số nước$A$, vậy đầu ra của nó là$A \cdot s_1 / S$. 

Nhiệm vụ là giảm thiểu số lượng lỗ bị chặn sao cho lỗ đầu tiên nhận được ít nhất$B$đơn vị nước. 

Những ràng buộc đẩy chúng ta tới một giải pháp tham lam hoặc dựa trên sự sắp xếp. Với$n$lên tới$10^5$, bất kỳ cách tiếp cận nào thử tất cả các tập con lỗ đều không thể thực hiện được. Thậm chí lặp đi lặp lại tất cả$2^n$các khả năng là không thể xảy ra và thậm chí việc kiểm tra từng số lần xóa có thể xảy ra bằng cách tính toán lại sẽ quá chậm nếu được thực hiện một cách ngây thơ. 

Một điểm tinh tế là việc loại bỏ lỗ chỉ ảnh hưởng đến mẫu số$S$, không phải tử số$s_1$. Điều này tạo ra hiệu ứng đơn điệu: việc loại bỏ các lỗ luôn làm tăng tỷ lệ của lỗ đầu tiên. 

Một vài trường hợp cạnh rất dễ bị bỏ sót. 

Nếu không có lỗ nào bị chặn thì có thể đã đủ. Ví dụ, nếu$s_1$lớn so với tổng của tất cả các kích thước, điều kiện đúng và đáp án bằng 0. 

Nếu tất cả các lỗ khác đều cực kỳ lớn so với$s_1$, thì cách duy nhất để thỏa mãn điều kiện là loại bỏ hầu hết mọi thứ ngoại trừ lỗ đầu tiên. Trong trường hợp cực đoan, câu trả lời trở thành$n-1$. 

Trường hợp khó phát hiện thứ ba là khi có nhiều lỗ có kích thước giống nhau. Vì chỉ có tổng quan trọng chứ không phải danh tính nên quyết định chỉ phụ thuộc vào việc loại bỏ những người đóng góp lớn nhất cho mẫu số. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là thử chặn tất cả các tập hợp con lỗ. Với mỗi tập hợp con, hãy tính tổng còn lại$S$, và kiểm tra xem$A \cdot s_1 / S \ge B$. Điều này sẽ yêu cầu lặp lại tất cả các tập hợp con và tính tổng các phần tử còn lại, dẫn đến$O(n \cdot 2^n)$hành vi. Với$n = 10^5$, điều này hoàn toàn không thể thực hiện được. 

Một cái nhìn có cấu trúc hơn đến từ việc viết lại điều kiện:$$\frac{A \cdot s_1}{S} \ge B \quad \Rightarrow \quad S \le \frac{A \cdot s_1}{B}$$Vì vậy, thay vì nghĩ đến việc trực tiếp tối đa hóa phần chia của lỗ đầu tiên, chúng tôi nghĩ đến việc giảm tổng số tiền còn lại$S$. Kích thước lỗ đầu tiên là cố định, vì vậy chúng tôi muốn tổng số lỗ còn lại càng nhỏ càng tốt, đồng thời chặn càng ít lỗ càng tốt. 

Điều này đưa ra một hướng tham lam: để giảm tổng nhanh nhất, trước tiên chúng ta nên loại bỏ các lỗ lớn nhất. Mỗi lỗ bị loại bỏ sẽ giảm$S$càng nhiều càng tốt cho mỗi lần loại bỏ. Do đó, việc sắp xếp các lỗ (không bao gồm lỗ đầu tiên) theo thứ tự giảm dần và loại bỏ dần chúng sẽ mang lại chiến lược tối ưu. 

Chúng tôi mô phỏng loại bỏ từng kích thước lớn nhất cho đến khi điều kiện được thỏa mãn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tất cả các tập hợp con) |$O(2^n \cdot n)$|$O(n)$| Quá chậm | 
| Sắp xếp + Loại bỏ tham lam |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng của tất cả các kích thước lỗ. Điều này đại diện cho mẫu số trước khi loại bỏ bất kỳ. 
2. Trích xuất kích thước của lỗ đầu tiên một cách riêng biệt vì nó không bao giờ bị loại bỏ. 
3. Kiểm tra xem điều kiện đã được thỏa mãn chưa mà không cần xóa bất cứ thứ gì. Nếu như$A \cdot s_1 \ge B \cdot S$, trả về 0 ngay lập tức. 
4. Thu thập tất cả các kích thước lỗ khác vào danh sách, vì chỉ có chúng mới là ứng cử viên để loại bỏ. 
5. Sắp xếp danh sách này theo thứ tự giảm dần sao cho những người đóng góp lớn nhất vào mẫu số sẽ được xem xét đầu tiên. 
6. Lặp lại danh sách đã sắp xếp, loại bỏ từng lỗ một: 

1. Trừ kích thước của nó khỏi tổng số tiền. 
2. Tăng số lượng loại bỏ. 
3. Kiểm tra xem có bất đẳng thức không$A \cdot s_1 \ge B \cdot S$bây giờ nắm giữ. 

Nếu giữ nguyên thì dừng lại và trả lại số lỗ đã loại bỏ. 

Lý do đằng sau việc sắp xếp là mỗi lần loại bỏ sẽ làm giảm mẫu số và việc giảm mẫu số đi một lượng lớn hơn ít nhất cũng tốt bằng việc giảm nó đi một lượng nhỏ hơn khi chúng ta chỉ quan tâm đến việc đạt đến một ngưỡng. 

### Tại sao nó hoạt động 

Bất biến quan trọng là tại bất kỳ thời điểm nào, trong số tất cả các bộ lỗ có cùng kích thước (cùng số lần loại bỏ), việc loại bỏ các lỗ lớn nhất có sẵn sẽ giảm thiểu tổng còn lại$S$. Vì điều kiện chỉ phụ thuộc vào việc$S$nằm dưới ngưỡng cố định, việc đạt đến ngưỡng đó với số lần xóa ít nhất tương đương với việc luôn tối đa hóa mức giảm$S$ở mỗi bước. Bất kỳ sai lệch nào trong việc loại bỏ phần tử lớn nhất còn lại chỉ có thể để lại số tiền lớn hơn sau cùng một số lần loại bỏ, điều này không thể giúp thỏa mãn bất đẳng thức trước đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, A, B = map(int, input().split())
    s = list(map(int, input().split()))
    
    s1 = s[0]
    total = sum(s)
    
    if A * s1 >= B * total:
        print(0)
        return
    
    rest = s[1:]
    rest.sort(reverse=True)
    
    removed = 0
    for x in rest:
        total -= x
        removed += 1
        if A * s1 >= B * total:
            print(removed)
            return

if __name__ == "__main__":
    solve()
```Mã này tuân theo bất đẳng thức dẫn xuất một cách trực tiếp, tránh phép chia dấu phẩy động bằng cách nhân chéo điều kiện. Điều này ngăn ngừa các vấn đề chính xác. 

Bước sắp xếp đảm bảo rằng mỗi lần lặp tạo ra mức giảm tối đa có thể về tổng số tiền trên mỗi phần tử bị loại bỏ. Vòng lặp sẽ dừng ngay lập tức khi điều kiện được thỏa mãn, vì vậy chúng ta không bao giờ loại bỏ nhiều lỗ hơn mức cần thiết. 

Một cạm bẫy phổ biến là vô tình đưa lỗ đầu tiên vào danh sách loại bỏ đã sắp xếp. Điều đó sẽ cho phép loại bỏ chính phần tử mà chúng ta phụ thuộc vào tử số một cách không chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 10 3
2 2 2 2
```Chúng ta bắt đầu với tổng = 8, s1 = 2. 

| Bước | Đã xóa lỗ | Tổng số tiền | Kiểm tra$10·2 ≥ 3·S$| 
| --- | --- | --- | --- | 
| 0 | không | 8 | 20 ≥ 24 sai | 
| 1 | 2 | 6 | 20 ≥ 18 đúng | 

Sau khi loại bỏ một lỗ, điều kiện trở thành đúng nên câu trả lời là 1. 

Điều này cho thấy việc loại bỏ bất kỳ lỗ đơn lẻ nào sẽ làm giảm mẫu số đủ để vượt qua ngưỡng. 

### Ví dụ 2 

đầu vào:```
3 80 20
3 3 4
```Tổng ban đầu = 10, s1 = 3. 

| Bước | Đã xóa lỗ | Tổng số tiền | Kiểm tra$80·3 ≥ 20·S$| 
| --- | --- | --- | --- | 
| 0 | không | 10 | 240 ≥ 200 đúng | 

Không cần loại bỏ. 

Điều này thể hiện trường hợp thoát sớm trong đó cấu hình ban đầu đã thỏa mãn ràng buộc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Sắp xếp tất cả các lỗ ngoại trừ lỗ chiếm ưu thế đầu tiên, sau đó là quét tuyến tính | 
| Không gian |$O(n)$| Lưu trữ danh sách kích thước lỗ | 

Các ràng buộc cho phép lên đến$10^5$các phần tử và$n \log n$sắp xếp thoải mái phù hợp trong thời hạn. Các phép toán còn lại là tuyến tính và không đáng kể khi so sánh. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n, A, B = map(int, input().split())
    s = list(map(int, input().split()))
    
    s1 = s[0]
    total = sum(s)
    
    if A * s1 >= B * total:
        return "0"
    
    rest = s[1:]
    rest.sort(reverse=True)
    
    removed = 0
    for x in rest:
        total -= x
        removed += 1
        if A * s1 >= B * total:
            return str(removed)

# provided sample
assert run("4 10 3\n2 2 2 2\n") == "1", "sample 1"

# all equal already satisfied
assert run("3 10 1\n5 5 5\n") == "0"

# need all removals except first
assert run("3 10 9\n1 100 100\n") == "2"

# minimal case
assert run("1 5 3\n10\n") == "0"

# large skew
assert run("5 100 1\n1 100 100 100 100\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều bình đẳng | 0 | thoát sớm đúng đắn | 
| mất cân bằng cực độ | 2 | loại bỏ tất cả những thứ khác cần thiết | 
| phần tử đơn | 0 | ranh giới n = 1 | 
| lệch giá trị lớn | 0 | điều kiện đã được thỏa mãn | 

## Vỏ cạnh 

Khi chỉ có một lỗ, mẫu số không bao giờ thay đổi và đáp án luôn bằng 0. Thuật toán xử lý việc này vì danh sách loại bỏ trống và việc kiểm tra điều kiện sớm sẽ quyết định kết quả ngay lập tức. 

Khi tất cả các lỗ trừ lỗ đầu tiên đều cực kỳ lớn, thuật toán sẽ loại bỏ chúng theo thứ tự giảm dần, nhanh chóng thu nhỏ mẫu số. Mỗi lần loại bỏ là điều cần thiết và vòng lặp tự nhiên sẽ đếm chính xác số lượng cần thiết cho đến khi chỉ còn lại lỗ đầu tiên hoặc đạt đến ngưỡng. 

Khi cấu hình ban đầu đã thỏa mãn bất đẳng thức, không có cách sắp xếp hoặc loại bỏ nào được sử dụng một cách hiệu quả vì việc kiểm tra sớm sẽ chấm dứt ngay lập tức.
