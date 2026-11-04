---
title: "CF 1174A - Ehab không thể trở thành Thanos"
description: "Chúng ta được cho một tập hợp các số nguyên có kích thước chính xác gấp đôi số n. Nhiệm vụ là sắp xếp lại các giá trị này thành một thứ tự mới sao cho nếu chia mảng đã được sắp xếp lại thành hai nửa liên tiếp có độ dài n thì tổng của nửa bên trái sẽ khác tổng của nửa bên phải."
date: "2026-06-13T09:43:02+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1174
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 563 (Div. 2)"
rating: 1000
weight: 1174
solve_time_s: 161
verified: false
draft: false
---

[CF 1174A - Ehab không thể trở thành Thanos](https://codeforces.com/problemset/problem/1174/A) 

**Đánh giá:** 1000 
**Tags:** thuật toán xây dựng, tham lam, sắp xếp 
**Thời gian giải:** 2m 41s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một tập hợp các số nguyên có kích thước chính xác gấp đôi số n. Nhiệm vụ là sắp xếp lại các giá trị này thành một thứ tự mới sao cho nếu chia mảng đã được sắp xếp lại thành hai nửa liên tiếp có độ dài n thì tổng của nửa bên trái sẽ khác tổng của nửa bên phải. Nếu không có sự sắp xếp lại như vậy, chúng tôi phải báo cáo sự thất bại. 

Điểm mấu chốt là chúng ta không được yêu cầu tối ưu hóa bất cứ điều gì ngoài điều kiện bất bình đẳng này. Chúng ta có thể tự do hoán vị một cách tùy ý, do đó cấu trúc của nghiệm phụ thuộc hoàn toàn vào cách chúng ta có thể tác động đến hai nửa tổng thông qua việc sắp xếp thứ tự. 

Ràng buộc n 1000 có nghĩa là kích thước mảng tối đa là 2000. Kích thước này đủ nhỏ để việc sắp xếp hoặc quét tuyến tính là không đáng kể và thậm chí cả cấu trúc O(n²) cũng có thể vượt qua. Phần quan trọng là lý luận về cấu trúc tổng hơn là hiệu quả tính toán. 

Trường hợp cạnh tinh tế xuất hiện khi tất cả các phần tử giống hệt nhau. Ví dụ: nếu mảng là [5, 5, 5, 5], thì bất kỳ phân vùng nào thành hai nửa đều tạo ra tổng bằng nhau vì cả hai nửa đều chứa cùng một tập hợp nhiều phần. Trong trường hợp này, không có giải pháp nào tồn tại. 

Một trường hợp cạnh khác phát sinh khi mảng có độ biến thiên rất nhỏ, chẳng hạn như [1, 1, 1, 2]. Một trực giác ngây thơ có thể gợi ý rằng một số cách đan xen thông minh vẫn có thể làm cho tổng bằng nhau, nhưng trên thực tế, chúng ta luôn có thể phá vỡ đẳng thức trừ khi tất cả các giá trị đều giống hệt nhau. 

Khó khăn cốt lõi là nhận biết khi nào tính đối xứng tạo ra sự bằng nhau bất kể hoán vị. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng hoán vị mảng và kiểm tra xem có tồn tại sự phân chia hợp lệ hay không. Có (2n)! hoán vị và thậm chí đánh giá một hoán vị là O(n), khiến điều này hoàn toàn không khả thi. 

Ngay cả khi chúng tôi giảm bớt các hoán vị lấy mẫu, chúng tôi vẫn thiếu cấu trúc. Quan sát thực tế là cách duy nhất mà tất cả các hoán vị đều thất bại là khi mọi phần tử đều giống hệt nhau. Trong trường hợp đó, mỗi nửa đều có tổng n · x bằng nhau nên sự bằng nhau là không thể tránh khỏi. 

Nếu tồn tại ít nhất hai giá trị phân biệt, chúng ta có thể cố ý xây dựng hai nửa có tổng khác nhau. Cách đơn giản nhất là sắp xếp mảng. Sau khi sắp xếp, các giá trị nhỏ nhất tập trung ở đầu và lớn nhất ở cuối. Việc tách sau n phần tử đảm bảo rằng nửa sau chứa ít nhất một phần tử lớn hơn bất kỳ phần tử nào trong nửa đầu hoặc ngược lại tùy thuộc vào phân phối. Điều này tạo ra sự mất cân bằng nghiêm trọng về số tiền. 

Cụ thể hơn, việc sắp xếp đảm bảo rằng chúng ta tối đa hóa sự tách biệt giữa các nửa. Nửa đầu thiên về các giá trị nhỏ hơn, nửa thứ hai thiên về các giá trị lớn hơn, điều này tạo ra một khoảng cách tổng. 

Vì vậy, vấn đề giảm xuống còn một kiểm tra đơn giản về tính đồng nhất, sau đó là sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Hoán vị Brute Force | O((2n)!) | O(n) | Quá chậm | 
| Sắp xếp + Xây dựng | O(n log n) | O(1) thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mảng và kiểm tra xem tất cả các phần tử có bằng nhau không. Nếu đúng như vậy, hãy ghi -1 vì mọi hoán vị đều tạo ra các nửa tổng giống nhau, khiến cho bất đẳng thức không thể xảy ra. 
2. Sắp xếp mảng theo thứ tự không giảm. Điều này tạo ra một cấu trúc trong đó các giá trị nhỏ được nhóm ở bên trái và các giá trị lớn ở bên phải. 
3. Xuất trực tiếp mảng đã sắp xếp. Việc chia thành hai nửa có kích thước n bây giờ sẽ so sánh nửa có trọng lượng thấp hơn với nửa có trọng lượng cao hơn. 
4. Chấm dứt. 

### Tại sao nó hoạt động

Việc sắp xếp tạo ra sự sắp xếp đơn điệu trong đó mọi phần tử ở nửa đầu đều nhỏ hơn hoặc bằng mọi phần tử ở nửa sau. Vì tồn tại ít nhất một bất đẳng thức nghiêm ngặt khi mảng không đồng nhất, nửa sau chứa tổng đóng góp lớn hơn. Điều này đảm bảo tổng của hai nửa không thể trùng nhau, bởi vì việc thay thế bất kỳ phần tử nào trong nửa đầu bằng một phần lớn hơn sẽ làm tăng tổng của nửa sau so với nửa đầu. 

Tính đúng đắn phụ thuộc vào thực tế là sự bình đẳng của tất cả các phần tử là cấu hình duy nhất loại bỏ mọi trật tự nghiêm ngặt có thể có giữa các nửa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    if len(set(a)) == 1:
        print(-1)
        return
    
    a.sort()
    print(*a)

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên đọc các phần tử n và 2n. Việc kiểm tra tính đồng nhất bằng cách sử dụng một bộ là rất quan trọng vì nó trực tiếp nắm bắt được trường hợp không thể duy nhất. Nếu không có nó, việc sắp xếp một mình vẫn sẽ tạo ra một hoán vị hợp lệ, nhưng chúng ta sẽ thất bại trong trường hợp các cạnh bằng nhau. 

Sắp xếp là bước mang tính xây dựng nhằm thực thi sự phân tách giữa các giá trị nhỏ và lớn. In nguyên mảng sau khi sắp xếp là đủ vì bài toán không yêu cầu hình thành một nửa một cách rõ ràng, chỉ đảm bảo rằng việc phân chia như vậy sẽ có tổng không bằng nhau. 

Một lỗi phổ biến là cố gắng xen kẽ các phần tử hoặc xây dựng các mẫu xen kẽ. Những điều này là không cần thiết vì chúng không cải thiện sự đảm bảo ngoài những gì việc phân loại đã cung cấp. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3
a = [1, 2, 2, 1, 3, 1]
```Mảng được sắp xếp:```
[1, 1, 1, 2, 2, 3]
```| Bước | Trạng thái mảng | Tổng nửa đầu | Tổng nửa sau | 
| --- | --- | --- | --- | 
| Đã sắp xếp | 1 1 1 2 2 3 | 3 | 7 | 

Nửa đầu chỉ chứa các giá trị nhỏ trong khi nửa sau chứa phần tử lớn nhất 3 và hai số 2. Điều này đảm bảo tổng thứ hai lớn hơn. 

### Ví dụ 2 

đầu vào:```
n = 2
a = [4, 4, 4, 4]
```| Bước | Trạng thái mảng | Tổng nửa đầu | Tổng nửa sau | 
| --- | --- | --- | --- | 
| Kiểm tra | tất cả đều bình đẳng | 8 | 8 | 

Vì tất cả các giá trị đều giống nhau nên mọi phân vùng đều mang lại số tiền bằng nhau. Không có sự sắp xếp lại nào có thể thay đổi điều này, vì vậy đầu ra là -1. 

Điều này xác nhận chế độ thất bại duy nhất là tính đồng nhất hoàn toàn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp thống trị thời gian chạy | 
| Không gian | O(1) thêm | Sắp xếp tại chỗ ngoài bộ nhớ đầu vào | 

Các ràng buộc cho phép tối đa 2000 phần tử, do đó việc sắp xếp rất nhanh. Giải pháp chạy thoải mái trong cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    if len(set(a)) == 1:
        print(-1)
        return
    a.sort()
    print(*a)

# provided sample
assert run("3\n1 2 2 1 3 1\n") != "-1"

# all equal
assert run("2\n5 5 5 5\n") == "-1", "all equal case"

# minimal n=1
assert run("1\n1 2\n") in ["1 2", "2 1"], "minimum case"

# already sorted
assert run("2\n1 2 3 4\n") == "1 2 3 4", "already sorted case"

# reverse order
assert run("2\n4 3 2 1\n") == "1 2 3 4", "reverse case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều bình đẳng | -1 | điều kiện bất khả thi | 
| n=1 trường hợp | bất kỳ đơn hàng hợp lệ nào | cấu trúc hợp lệ nhỏ nhất | 
| đầu vào được sắp xếp | đầu ra được sắp xếp | ổn định công trình | 
| đầu vào đảo ngược | đầu ra được sắp xếp | tính đúng đắn theo hoán vị | 

## Vỏ cạnh 

Trường hợp cạnh quan trọng nhất là khi tất cả các phần tử giống hệt nhau. Đối với một đầu vào như:```
3
7 7 7 7 7 7
```Thuật toán phát hiện`len(set(a)) == 1`và ngay lập tức xuất ra -1. Nếu chúng ta bỏ qua bước kiểm tra này và in mảng, cả hai nửa sẽ có tổng là 3 × 7 = 21, vi phạm điều kiện. 

Đối với trường hợp gần đồng nhất như:```
3
1 1 1 1 1 2
```Kích thước đặt lớn hơn 1 nên ta tiến hành sắp xếp:```
1 1 1 1 1 2
```Một nửa số tiền trở thành: 

nửa đầu = 3, nửa sau = 4, vì vậy điều kiện đúng. Điều này cho thấy rằng ngay cả một sai lệch so với tính đồng nhất cũng đủ để đảm bảo sự phân chia hợp lệ sau khi sắp xếp. 

Hành vi này xác nhận rằng việc xây dựng chỉ thất bại trong trường hợp đối xứng hoàn toàn và thành công trong trường hợp khác.
