---
title: "CF 105416F - Dây chuyền ấp trứng"
description: "Chúng ta được cung cấp một tập hợp các vị trí trên trục số nơi đặt trứng. Chúng ta cũng được phép đặt một số lượng đèn nhiệt cố định, mỗi đèn có tọa độ nguyên."
date: "2026-06-23T04:41:56+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105416
codeforces_index: "F"
codeforces_contest_name: "UTPC Contest 10-11-24 Div. 2 (Beginner)"
rating: 0
weight: 105416
solve_time_s: 78
verified: false
draft: false
---

[CF 105416F - Dây chuyền ươm tạo](https://codeforces.com/problemset/problem/105416/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các vị trí trên trục số nơi đặt trứng. Chúng ta cũng được phép đặt một số lượng đèn nhiệt cố định, mỗi đèn có tọa độ nguyên. Mỗi quả trứng được “phục vụ” bởi chiếc đèn gần nhất và chất lượng của cấu hình được đo bằng khoảng cách tối đa từ bất kỳ quả trứng nào đến chiếc đèn gần nhất. 

Nhiệm vụ là đặt tối đa$k$đèn sao cho khoảng cách tối đa này càng nhỏ càng tốt. 

Một cách hữu ích để nghĩ về điều này là mỗi chiếc đèn bao phủ những quả trứng ở vùng lân cận của nó và mỗi quả trứng phải nằm trong bán kính nào đó của ít nhất một chiếc đèn. Chúng tôi đang cố gắng giảm thiểu bán kính nhỏ nhất cho phép che phủ tối đa tất cả trứng bằng cách sử dụng$k$các trung tâm. 

Các ràng buộc rất lớn, có thể lên tới$5 \cdot 10^5$vị trí, loại trừ mọi giải pháp cố gắng đánh giá tất cả các vị trí hoặc mô phỏng khoảng cách cho mọi cấu hình. Bất cứ điều gì bậc hai hoặc thậm chí$O(nk)$sẽ không hoạt động. Chúng ta cần ít nhất$O(n \log n)$, tiêu biểu$O(n \log \max a_i)$hoặc$O(n \log n)$. 

Một cách tiếp cận đơn giản sẽ thử mọi vị trí đặt đèn có thể giữa các quả trứng hoặc giữa chúng và tính toán phạm vi bao phủ, nhưng điều đó ngay lập tức thất bại vì số lượng cấu hình ứng cử viên tăng lên một cách tổ hợp. 

Một trường hợp thất bại tinh vi đối với trực giác tham lam xuất hiện khi các quả trứng có khoảng cách không đều nhau. Ví dụ, nếu hầu hết các quả trứng tập trung lại nhưng một quả ở xa, việc phân chia bằng nhau một cách ngây thơ có thể đặt đèn kém. 

Đầu vào như:```
5 2
0 1 2 100 101
```Cách tiếp cận “chia đều theo số lượng” ngây thơ có thể đặt các đèn gần ranh giới cụm chỉ số 2 và 101 không chính xác và bỏ lỡ việc phân nhóm tối ưu. Giải pháp đúng phụ thuộc vào khoảng cách, không tính. 

## Phương pháp tiếp cận 

Cách giải thích thô bạo là xem xét bán kính$R$và hỏi liệu chúng ta có thể đặt tối đa$k$đèn để mọi quả trứng đều ở trong khoảng cách$R$của một chiếc đèn nào đó. Nếu chúng ta có thể kiểm tra điều này một cách hiệu quả, chúng ta có thể tìm kiếm nhị phân trên$R$. 

Đối với một cố định$R$, chúng ta có thể tưởng tượng mỗi chiếc đèn được đặt ở một vị trí nào đó bao phủ một khoảng chiều dài$2R$. Vì tất cả trứng bên trong$R$của một đèn phải nằm trong một khoảng như vậy, mỗi đèn bao phủ một cách hiệu quả một đoạn liền kề của các vị trí trứng đã được sắp xếp. 

Điều này dẫn đến một cuộc kiểm tra tham lam: bắt đầu từ quả trứng không được che chắn ngoài cùng bên trái, đặt đèn một cách tối ưu để bao phủ càng nhiều trứng càng tốt trong khoảng cách$R$, và lặp lại. Điều này là tối ưu vì việc đặt đèn càng xa càng tốt trong phạm vi tầm phủ sóng cho phép luôn chiếm ưu thế hơn các vị trí trước đó. 

Phiên bản brute-force thử tất cả các vị trí là không khả thi vì số lượng tập hợp con của các điểm vị trí là theo cấp số nhân. Thông tin chi tiết quan trọng là đối với bán kính cố định, cấu trúc sẽ trở thành khoảng bao phủ trên một đường được sắp xếp, có thể giải được theo tham lam trong thời gian tuyến tính. 

Do đó, chúng tôi chuyển đổi một vấn đề tối ưu hóa thành một vấn đề quyết định và sau đó tìm kiếm câu trả lời nhị phân. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Vị trí vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Tìm kiếm nhị phân + Kiểm tra tham lam | O(n log V) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Bước 1: Sắp xếp vị trí trứng 

Việc sắp xếp là cần thiết vì phạm vi bao phủ chỉ có ý nghĩa theo thứ tự dọc theo dòng. Nếu không sắp xếp, chúng ta không thể suy luận về các phân đoạn liền kề. 

### Bước 2: Định nghĩa hàm`can(R)`Chúng tôi kiểm tra xem bán kính$R$sử dụng tối đa là đủ$k$đèn. 

### Bước 3: Quá trình bao che tham lam 

Bắt đầu từ quả trứng ngoài cùng bên trái chưa được che phủ. 

Lấy quả trứng đó vào vị trí$x$. Đặt đèn càng xa càng tốt trong khi vẫn che được$x$, có nghĩa là đặt nó ở$x + R$. Đèn này sau đó sẽ bao phủ tất cả trứng lên đến$x + 2R$. 

Sự lựa chọn này là tối ưu vì bất kỳ đèn nào che$x$phải nằm trong$[x - R, x + R]$và dịch chuyển nó sang phải chỉ làm tăng phạm vi bao phủ sang bên phải mà không làm mất phạm vi bao phủ của$x$. 

### Bước 4: Nhảy qua những quả trứng đã phủ kín 

Sau khi đặt đèn, hãy đưa con trỏ tới quả trứng đầu tiên phía sau$x + 2R$. Điều này mô phỏng việc tiêu thụ toàn bộ phân khúc trong một thao tác. 

### Bước 5: Đếm số đèn đã sử dụng 

Nếu số lượng đèn cần thiết vượt quá$k$, sau đó$R$là quá nhỏ. 

### Bước 6: Đáp án tìm kiếm nhị phân 

Tìm kiếm tối thiểu$R$từ 0 đến khoảng cách tối đa có thể giữa các quả trứng. Mỗi kiểm tra chạy trong thời gian tuyến tính. 

### Tại sao nó hoạt động 

Thuật toán dựa trên một tính chất đơn điệu: nếu bán kính$R$hoạt động, bất kỳ bán kính lớn hơn cũng hoạt động. Điều này cho phép tìm kiếm nhị phân. Lớp phủ tham lam đảm bảo rằng đối với bất kỳ cố định nào$R$, chúng tôi giảm thiểu số lượng đèn cần thiết bằng cách luôn che càng xa bên phải càng tốt ở mỗi bước. Điều này tránh lãng phí đèn khi phủ sóng một phần dưới mức tối ưu và đảm bảo rằng nếu chiến lược tham lam cần nhiều hơn$k$đèn, không có vị trí thay thế nào có thể làm tốt hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def can(a, k, r):
    n = len(a)
    i = 0
    used = 0

    while i < n:
        used += 1
        if used > k:
            return False

        start = a[i]
        cover_end = start + 2 * r

        while i < n and a[i] <= cover_end:
            i += 1

    return True

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    a.sort()

    lo, hi = 0, a[-1] - a[0]

    while lo < hi:
        mid = (lo + hi) // 2
        if can(a, k, mid):
            hi = mid
        else:
            lo = mid + 1

    print(lo)

if __name__ == "__main__":
    solve()
```Bước sắp xếp đảm bảo tất cả các khoảng tham lam đều liền kề nhau. các`can`là thủ tục quyết định cốt lõi và nó nhóm các quả trứng thành các khoảng kích thước tối thiểu.$2R$. Tìm kiếm nhị phân sau đó tìm thấy bán kính khả thi nhỏ nhất. 

Một điểm tinh tế là vị trí đèn không cần phải lưu trữ rõ ràng. Chúng ta chỉ cần khoảng bao phủ được tạo ra bằng cách đặt nó một cách tối ưu so với quả trứng ngoài cùng bên trái. 

Một lỗi phổ biến khác là sử dụng$R$trực tiếp dưới dạng độ dài khoảng thay vì$2R$. Đèn ở vị trí$x+R$bao gồm$[x, x+2R]$, không$[x, x+R]$. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 20
0 5 7
```Đã sắp xếp mảng rồi`[0, 5, 7]`. 

Chúng tôi kiểm tra tính khả thi cho bán kính. 

| Bước | tôi | vị trí | đèn đã qua sử dụng | kết thúc bảo hiểm | 
| --- | --- | --- | --- | --- | 
| bắt đầu | 0 | 0 | 1 | 0 + 2R | 
| di chuyển | 1 | 5 | 1 | 0 + 2R | 
| di chuyển | 2 | 7 | 1 | 0 + 2R | 
| kết thúc | 3 | - | 1 | - | 

Để đủ lớn$R$, một đèn là đủ. Nếu như$R = 1$, phạm vi phủ sóng là 2, vì vậy đèn đầu tiên chỉ bao phủ 0, cần một đèn khác cho 5 và 7, cho 2 đèn. Tìm kiếm nhị phân tìm giá trị nhỏ nhất$R$giữ đèn trong giới hạn. 

### Ví dụ 2 

đầu vào:```
5 2
-2 -1 0 1 4
```Đã sắp xếp:`[-2, -1, 0, 1, 4]`Hãy thử bán kính nhỏ, nói$R = 1$. 

| Bước | tôi | bắt đầu | bìa_end | đã qua sử dụng | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | -2 | 0 | 1 | 
| 2 | 3 | 1 | 3 | 2 | 
| 3 | 4 | 4 | - | 3 (thất bại) | 

Chúng ta cần 3 chiếc đèn, vậy$R = 1$là không thể thực hiện được. 

Vì$R = 2$: 

| Bước | tôi | bắt đầu | bìa_end | đã qua sử dụng | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | -2 | 2 | 1 | 
| 2 | 4 | 4 | 6 | 2 | 

Bây giờ nó hoạt động với 2 đèn, phù hợp$k$. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log V)$| Sắp xếp cộng với tìm kiếm nhị phân theo bán kính; mỗi lần kiểm tra tính khả thi đều tuyến tính | 
| Không gian |$O(n)$| Lưu trữ cho các vị trí được sắp xếp | 

Phạm vi giá trị$V$tùy thuộc vào$2 \cdot 10^9$, nhưng tìm kiếm nhị phân chỉ cần khoảng 31 lần lặp. Với$n = 5 \cdot 10^5$, điều này thoải mái phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from math import inf

    # Re-implement solution inline for testing
    def can(a, k, r):
        i = 0
        used = 0
        n = len(a)
        while i < n:
            used += 1
            if used > k:
                return False
            start = a[i]
            end = start + 2 * r
            while i < n and a[i] <= end:
                i += 1
        return True

    n, k, *rest = map(int, inp.split())
    a = rest
    a.sort()

    lo, hi = 0, a[-1] - a[0]
    while lo < hi:
        mid = (lo + hi) // 2
        if can(a, k, mid):
            hi = mid
        else:
            lo = mid + 1

    return str(lo)

# provided samples
assert run("3 2\n0 5 7") == "1", "sample 1"
assert run("5 2\n-2 1 0 -1 4") == "2", "sample 2"

# custom cases
assert run("1 1\n10") == "0", "single egg"
assert run("2 1\n0 100") == "50", "one lamp must cover both ends"
assert run("4 4\n1 2 3 4") == "0", "one lamp per egg"
assert run("6 2\n0 1 2 100 101 102") == "1", "two clusters"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 quả trứng | 0 | trường hợp cơ bản tầm thường | 
| hai điểm xa, 1 đèn | bán kính trung điểm | tính đúng đắn của suy luận khoảng | 
| k = n | 0 | mỗi quả trứng có đèn riêng | 
| hai cụm chặt chẽ | hành vi phân chia bán kính nhỏ | tham lam nhóm đúng đắn | 

## Vỏ cạnh 

Một đầu vào trứng như`n = 1`luôn trả về 0 vì không tồn tại khoảng cách. Thuật toán xử lý việc này vì phạm vi tìm kiếm nhị phân bị thu gọn và`can(0)`ngay lập tức trả về true với một đèn. 

Trường hợp tất cả các quả trứng đều cách đều nhau, chẳng hạn như`[1,2,3,4,5]`, buộc thuật toán phải chứng minh rằng việc nhóm hoàn toàn theo khoảng cách chứ không phải theo số lượng. Vì`k = 2`, quy trình tham lam tạo thành hai phân đoạn liền kề và không bao giờ cố gắng trộn các phần tử ở xa nhau, tạo ra sự phân chia tối ưu. 

Một ngoại lệ được phân tách rộng rãi, chẳng hạn như`[0,1,2,3,100000]`, kiểm tra xem chiến lược tham lam không lãng phí đèn ở những khu vực đông đúc. Vỏ đèn đầu tiên`[0..3]`, cái thứ hai chỉ bao phủ phần ngoại lệ và bất kỳ nỗ lực nào để dịch chuyển đèn đầu tiên sang phải sẽ làm giảm phạm vi bao phủ ở bên trái, điều mà bất biến cấm.
