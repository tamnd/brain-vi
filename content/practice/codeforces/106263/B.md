---
title: "CF 106263B - \u5c0f\u6a58\u7684\u9b54\u6cd5\u5b9d\u77f3"
description: "Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Trong mỗi viên đá, chúng ta nhận được một danh sách các số nguyên đại diện cho “sức mạnh ma thuật” của một bộ sưu tập đá. Đối với mỗi cặp đá riêng biệt không có thứ tự, chúng tôi tính toán giá trị AND theo từng bit của chúng rồi tính tổng các kết quả này trên tất cả các cặp."
date: "2026-06-18T23:18:04+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106263
codeforces_index: "B"
codeforces_contest_name: "2025 \u534e\u5357\u5e08\u8303\u5927\u5b66\u201c\u5353\u8d8a\u6559\u80b2\u676f\u201d\u7b97\u6cd5\u4e0e\u7a0b\u5e8f\u8bbe\u8ba1\u7ade\u8d5b\uff08\u65b0\u751f\u8d5b\uff09"
rating: 0
weight: 106263
solve_time_s: 52
verified: true
draft: false
---

[CF 106263B - \u5c0f\u6a58\u7684\u9b54\u6cd5\u5b9d\u77f3](https://codeforces.com/problemset/problem/106263/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Trong mỗi viên đá, chúng ta nhận được một danh sách các số nguyên đại diện cho “sức mạnh ma thuật” của một bộ sưu tập đá. Đối với mỗi cặp đá riêng biệt không có thứ tự, chúng tôi tính toán giá trị AND theo từng bit của chúng rồi tính tổng các kết quả này trên tất cả các cặp. 

Vì vậy, nhiệm vụ hoàn toàn mang tính tổ hợp: mỗi cặp đóng góp một giá trị được xác định bởi các bit tập hợp chung giữa hai số. Đầu ra của mỗi trường hợp thử nghiệm là tổng của tất cả các cặp. 

Thách thức chính về cấu trúc là số lượng cặp tăng bậc hai với n. Khi n đạt 200.000 trong một thử nghiệm, số cặp sẽ ở mức 2×10^10, điều này ngay lập tức loại trừ mọi phép liệt kê cặp trực tiếp. Ngay cả với nhiều trường hợp thử nghiệm, tổng n có thể đạt tới 400.000, do đó, mọi giải pháp đều phải gần với tuyến tính hoặc logarit tuyến tính. 

Một trường hợp thất bại tinh tế đối với lý luận ngây thơ là giả sử rằng AND theo cặp hoạt động giống như trung bình số học hoặc có thể được tổng hợp mà không xem xét tính độc lập của bit. Ví dụ: nếu tất cả các số đều giống hệt nhau, hãy nói`[7, 7, 7]`, mỗi cặp đóng góp`7 & 7 = 7`, vậy câu trả lời là`3 * 7 = 21`. Một nỗ lực ngây thơ đếm nhầm các đóng góp cho mỗi phần tử thay vì mỗi cặp có thể dễ dàng đếm thừa hoặc đếm thiếu bằng cách trộn lẫn các cặp có thứ tự và không có thứ tự. 

Một cạm bẫy khác là quên rằng các phép toán theo bit sẽ phân hủy rõ ràng trên mỗi bit, nhưng chỉ khi mỗi bit được xử lý độc lập. Bất kỳ cách tiếp cận nào cố gắng xây dựng lại các giá trị AND mà không có lý do trên mỗi bit đều có xu hướng sụp đổ dưới những ràng buộc lớn. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực sẽ lặp lại trên tất cả các cặp`(i, j)`với`i < j`và tính toán`(a[i] & a[j])`, tích lũy kết quả. Điều này đúng vì nó trực tiếp tuân theo định nghĩa của vấn đề. Tuy nhiên, chi phí của nó tỷ lệ thuận với số lượng cặp, trong trường hợp xấu nhất là`n(n-1)/2`. Với`n = 2 × 10^5`, điều này trở nên hoàn toàn không thể thực hiện được. 

Cấu trúc của bitwise AND gợi ý sự phân tách: thay vì tính toán các giá trị đầy đủ trên mỗi cặp, chúng ta có thể xem xét từng vị trí bit một cách độc lập. Một chút đóng góp`2^k`đến tổng AND cuối cùng của một cặp khi và chỉ khi cả hai số trong cặp đều có tập bit đó. Điều này chuyển đổi vấn đề từ lý luận về giá trị sang lý luận về số lượng bit. 

Đối với một bit cố định`k`, giả định`cnt_k`các số có tập bit này. Bất kỳ cặp nào được hình thành từ những`cnt_k`những con số đóng góp`2^k`để trả lời. Số cặp như vậy là`cnt_k * (cnt_k - 1) / 2`. Tổng hợp tất cả các bit sẽ cho kết quả cuối cùng. 

Điều này hoạt động vì bitwise AND có tính cộng trên các vị trí bit độc lập và không tồn tại sự mang hoặc tương tác giữa các bit. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Đếm Bit | O(n · 31) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp kiểm thử một cách độc lập và tính toán câu trả lời bằng cách sử dụng tính năng đếm trên mỗi bit. 

1. Khởi tạo một cấu trúc mảng hoặc vòng lặp để đếm xem mỗi bit có bao nhiêu số. Từ`a[i] ≤ 10^8`, chúng ta chỉ cần bit tối đa 30. 
2. Với mỗi số trong mảng, kiểm tra từng vị trí bit. Nếu bit được đặt, hãy tăng bộ đếm tương ứng. Điều này xây dựng một bảng tần số về số lượng chứa mỗi bit. 
3. Đối với từng vị trí bit`k`, hãy tính xem có thể tạo được bao nhiêu cặp từ các số có tập bit đó. Nếu như`cnt_k`số chứa bit`k`, số cặp hợp lệ là`cnt_k * (cnt_k - 1) // 2`. 
4. Mỗi cặp như vậy đóng góp`2^k`đến số tiền cuối cùng. Nhân và thêm phần đóng góp này vào câu trả lời. 
5. Xuất kết quả tích lũy cho test case. 

Lý do chúng ta đếm các cặp sau khi đếm tần số là vì sự hình thành cặp chỉ phụ thuộc vào thành viên trong tập hợp số có một bit chứ không phụ thuộc vào thứ tự hay tương tác với các bit khác. 

### Tại sao nó hoạt động 

Mỗi cặp đóng góp vào tổng cuối cùng một cách độc lập cho từng bit trong đó cả hai số đều có chung số 1. Nếu chúng ta cố định một vị trí bit thì phần đóng góp của tất cả các cặp được xác định hoàn toàn bằng số lượng số bao gồm bit đó. Bởi vì các vị trí bit khác nhau không bao giờ can thiệp, nên việc tính tổng các đóng góp trên các bit sẽ tái tạo lại tổng AND đầy đủ trên tất cả các cặp chính xác một lần trên mỗi kết hợp cặp bit. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        cnt = [0] * 31  # enough for values up to 1e9

        for x in a:
            b = 0
            while x:
                if x & 1:
                    cnt[b] += 1
                x >>= 1
                b += 1

        ans = 0
        for k in range(31):
            c = cnt[k]
            if c > 1:
                ans += (c * (c - 1) // 2) * (1 << k)

        print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp sử dụng mảng tần số trên mỗi bit`cnt`. Đối với mỗi số, chúng tôi phân tách nó bằng cách sử dụng dịch chuyển bit thay vì liên tục che dấu từng chỉ mục bit, điều này giúp việc triển khai đơn giản và tránh được chi phí bổ sung. 

Một lỗi triển khai phổ biến là lặp lại các bit không chính xác và dịch chuyển số ban đầu một cách triệt để mà không cần quan tâm. Ở đây, chúng ta xây dựng lại các vị trí bit bằng cách sử dụng một vòng lặp dịch chuyển`x`, vì vậy mỗi số được tiêu thụ một cách an toàn mà không ảnh hưởng đến những số khác. 

Công thức kết hợp`c * (c - 1) // 2`phải được tính toán theo số học an toàn 64-bit. Python xử lý các số nguyên lớn một cách tự nhiên nên việc tràn dữ liệu không phải là vấn đề đáng lo ngại. 

## Ví dụ đã hoạt động 

Xem xét đầu vào:```
1
3
1 2 3
```Chúng tôi theo dõi số lượng bit: 

| Bước | Số | Đếm bit 0 | Đếm bit 1 | Trạng thái một phần | 
| --- | --- | --- | --- | --- | 
| 1 | 1 (01) | 1 | 0 | [1,0] | 
| 2 | 2 (10) | 1 | 1 | [1,1] | 
| 3 | 3 (11) | 2 | 2 | [2,2] | 

Bây giờ tính toán đóng góp: 

Đối với bit 0:`cnt = 2`, cặp = 1, đóng góp =`1 * 1 = 1`Đối với bit 1:`cnt = 2`, cặp = 1, đóng góp =`2 * 1 = 2`Câu trả lời cuối cùng = 3. 

Dấu vết này cho thấy mỗi bit đóng góp độc lập và chồng chéo giữa các số được tích lũy một cách chính xác. 

Bây giờ hãy xem xét:```
1
4
5 5 5 5
```Nhị phân 5 =`101`. 

| Bước | Số | Bit 0 | Bit 2 | Tiểu bang | 
| --- | --- | --- | --- | --- | 
| 1 | 5 | 1 | 1 | [1,1] | 
| 2 | 5 | 2 | 2 | [2,2] | 
| 3 | 5 | 3 | 3 | [3,3] | 
| 4 | 5 | 4 | 4 | [4,4] | 

Mỗi bit có`cnt = 4`, vậy cặp = 6. 

Đóng góp bit 0 = 6 * 1 = 6 

Đóng góp bit 2 = 6 * 4 = 24 

Tổng cộng = 30 

Điều này phù hợp với thực tế là cả 6 cặp đều tạo ra`(5 & 5) = 5`, vậy tổng số là`6 * 5 = 30`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(T · n · B) | Mỗi số được xử lý tối đa B = 31 bit | 
| Không gian | O(B) | Chỉ mảng tần số cho bit | 

Các ràng buộc cho phép tổng số phần tử lên tới 4×10^5 trong tất cả các trường hợp thử nghiệm. Với khoảng 31 thao tác cho mỗi phần tử, tổng công việc là khoảng 1,2×10^7 thao tác, dễ dàng phù hợp với giới hạn thời gian trong Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout

    data = inp.strip().split()
    it = iter(data)
    t = int(next(it))
    out = []

    for _ in range(t):
        n = int(next(it))
        arr = [int(next(it)) for _ in range(n)]

        cnt = [0] * 31
        for x in arr:
            b = 0
            while x:
                if x & 1:
                    cnt[b] += 1
                x >>= 1
                b += 1

        ans = 0
        for k in range(31):
            c = cnt[k]
            ans += (c * (c - 1) // 2) * (1 << k)

        out.append(str(ans))

    return "\n".join(out)

# provided sample
assert run("1\n3\n1 2 3") == "3"

# minimum size
assert run("1\n1\n7") == "0"

# all equal
assert run("1\n4\n5 5 5 5") == "30"

# no shared bits
assert run("1\n3\n1 2 4") == "0"

# mixed
assert run("2\n2\n1 3\n3\n7 7 7") == "1\n21"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | 0 | không có cặp nào tồn tại | 
| tất cả đều bình đẳng | 30 | tính chính xác của việc đếm cặp tổ hợp | 
| bit rời rạc | 0 | VÀ không lan truyền | 
| trường hợp hỗn hợp | đa dạng | xử lý nhiều bài kiểm tra và tính chính xác | 

## Vỏ cạnh 

Một bài kiểm tra đơn yếu tố như`n = 1`cho biết liệu việc triển khai có giả định không chính xác ít nhất một cặp tồn tại hay không. Trong trường hợp này, số bit có thể khác 0, nhưng công thức cặp chính xác cho kết quả bằng 0 vì`cnt * (cnt - 1) / 2`là số không. 

Đối với một đầu vào như`3 1 2 4`, mỗi số có các bit rời nhau nên mọi AND đều bằng 0. Phương pháp đếm bit đặt số lượng mỗi bit thành chính xác một, tạo ra mức đóng góp bằng 0 cho mỗi bit, phù hợp với kết quả mong đợi. 

Đối với một mảng thống nhất như`5 5 5 5`, mỗi bit xuất hiện bốn lần và thuật toán được tính`6`cặp trên mỗi bit, tái tạo chính xác thực tế là tất cả các cặp đều có AND giống nhau bằng 5.
