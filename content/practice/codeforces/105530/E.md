---
title: "CF 105530E - Nice (Phiên bản trung bình)"
description: "Chúng ta đang xem xét các số được hình thành chỉ bằng hai chữ số, 6 và 9. Bất kỳ số hợp lệ nào đều là số “đẹp” nếu mọi vị trí đều là một trong hai chữ số này."
date: "2026-06-23T22:59:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105530
codeforces_index: "E"
codeforces_contest_name: "Metropolitan University Inter University Programming Contest - Sylhet Division 2024"
rating: 0
weight: 105530
solve_time_s: 50
verified: true
draft: false
---

[CF 105530E - Nice (Phiên bản trung bình)](https://codeforces.com/problemset/problem/105530/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta đang xem xét các số được hình thành chỉ bằng hai chữ số, 6 và 9. Bất kỳ số hợp lệ nào đều là số “đẹp” nếu mọi vị trí đều là một trong hai chữ số này. Nhiệm vụ không phải là xây dựng một cái gì đó phức tạp từ chúng mà là trả lời nhiều truy vấn có dạng: có bao nhiêu số đẹp như vậy nằm trong một khoảng số nhất định$[l, r]$. 

Vì vậy, vấn đề giảm xuống còn việc hiểu một vũ trụ số rất cụ thể: tất cả các chuỗi trên bảng chữ cái$\{6, 9\}$, được hiểu là số nguyên không có giới hạn hàng đầu ngoài việc lựa chọn chữ số. Đối với mỗi truy vấn, chúng tôi muốn đếm xem có bao nhiêu số đặc biệt này nằm giữa hai giới hạn. 

Quan sát cấu trúc quan trọng là tập hợp tất cả các số đẹp cực kỳ nhỏ so với phạm vi các số nguyên lên đến$10^{18}$. Mỗi vị trí chữ số độc lập có 2 lựa chọn, vì vậy với độ dài cố định$i$, có chính xác$2^i$những con số như vậy. Tổng tất cả các độ dài lên tới 18, tổng số là$2^1 + 2^2 + \dots + 2^{18}$, chỉ có vài trăm nghìn phần tử. Điều này làm cho việc liệt kê thô bạo trở nên hợp lý. 

Các ràng buộc ngụ ý rằng bất kỳ giải pháp nào lặp qua tất cả các số nguyên lên đến$10^{18}$mỗi truy vấn là không thể, vì điều đó sẽ lên tới$10^{18}$hoạt động. Ngay cả việc quét tuyến tính cho mỗi truy vấn cũng sẽ thất bại ngay lập tức. Mặt khác, việc lặp đi lặp lại nhiều nhất là xung quanh$3 \cdot 10^5$các giá trị được tính toán trước là không đáng kể, thậm chí đối với nhiều trường hợp thử nghiệm. 

Một trường hợp cạnh tinh tế phát sinh từ việc xử lý ranh giới khi$l = 1$hoặc khi nào$l$bản thân nó là một con số đẹp. Ví dụ: nếu chúng ta đếm số$\le r$và trừ các số$< l$, thì sẽ xảy ra lỗi từng lỗi một nếu chúng ta vô tình loại trừ hoặc đếm gấp đôi các giá trị chính xác bằng$l$. Một trường hợp đặc biệt khác là xử lý các số dưới dạng chuỗi so với số nguyên; các số 0 đứng đầu không liên quan vì chúng tôi chỉ tạo các chuỗi chữ số có độ dài cố định bắt đầu từ độ dài 1. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực trực tiếp sẽ là lặp lại mọi số nguyên từ$l$ĐẾN$r$và kiểm tra xem mỗi số có chỉ chứa các chữ số 6 và 9 hay không. Việc kiểm tra này là tuyến tính về số chữ số, vì vậy đối với một số thì nó là$O(18)$. Tuy nhiên, độ dài phạm vi có thể lên tới$10^{18}$, khiến điều này hoàn toàn không thể thực hiện được. Ngay cả một truy vấn duy nhất cũng có thể yêu cầu$10^{18}$séc. 

Quan sát quan trọng là chúng ta thực sự không cần phải kiểm tra các số tùy ý. Toàn bộ tập hợp hợp lệ được xác định theo cấu trúc: mỗi độ dài đóng góp chính xác một tập hợp các chữ số nhị phân đầy đủ. Điều này có nghĩa là chúng ta có thể tính toán trước tất cả các số hợp lệ một lần, lưu trữ chúng theo thứ tự được sắp xếp và trả lời các truy vấn bằng tìm kiếm nhị phân. 

Chúng tôi tạo ra tất cả các số đẹp bằng cách quay lui hoặc bitmasking theo độ dài từ 1 đến 18. Mỗi bước xây dựng nối thêm 6 hoặc 9, tạo thành một cây nhị phân hoàn chỉnh có kích thước$2^i$ở độ sâu$i$. Sau khi thu thập tất cả các giá trị, chúng tôi sắp xếp chúng. Sau đó, mỗi truy vấn sẽ trở thành một bài toán đếm tiền tố tiêu chuẩn: đếm xem có bao nhiêu giá trị được tính toán trước nằm trong$[l, r]$sử dụng hai tìm kiếm nhị phân. 

Quá trình chuyển đổi từ lực lượng vũ phu sang tối ưu xuất phát từ quan điểm thay đổi: thay vì quét dòng số nguyên, chúng tôi liệt kê trực tiếp cấu trúc tổ hợp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O((r-l+1)\cdot 18)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(2^{18} \log 2^{18})$tiền xử lý +$O(\log 2^{18})$mỗi truy vấn |$O(2^{18})$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo ra tất cả các số đẹp bằng cách xây dựng các chuỗi chữ số có độ dài từ 1 đến 18. Mỗi vị trí phân nhánh thành hai khả năng, 6 hoặc 9. Điều này đảm bảo mọi số hợp lệ được tạo ra chính xác một lần vì mỗi chuỗi chữ số tương ứng với chính xác một số nguyên. 
2. Trong quá trình tạo, chuyển đổi từng chuỗi chữ số được xây dựng thành một số nguyên và lưu trữ nó trong một mảng. Bước này là cần thiết vì sau này chúng ta sẽ cần sắp xếp theo giá trị số chứ không phải thứ tự xây dựng từ điển. 
3. Sau khi tạo xong, sắp xếp mảng toàn số đẹp. Sắp xếp sắp xếp cấu trúc để có thể áp dụng tìm kiếm nhị phân. Nếu không sắp xếp, chúng ta sẽ mất khả năng trả lời các truy vấn phạm vi một cách hiệu quả. 
4. Đối với mỗi truy vấn$[l, r]$, tính số lượng giá trị hợp lệ$\le r$sử dụng tìm kiếm nhị phân, sau đó trừ đi số giá trị hợp lệ$< l$. Điều này chuyển đổi một truy vấn phạm vi thành hai truy vấn tiền tố. 
5. Trả về kết quả chênh lệch cho truy vấn đó. 

### Tại sao nó hoạt động 

Bất biến chính là mảng được tính toán trước chứa mọi và chỉ số đẹp hợp lệ đúng một lần và được sắp xếp theo thứ tự số tăng dần. Do đó, tìm kiếm nhị phân xác định chính xác số lượng tiền tố: tất cả các phần tử$\le x$tạo thành một tiền tố liền kề của danh sách được sắp xếp. Trừ hai số tiền tố sẽ tách biệt chính xác các phần tử nằm trong$[l, r]$, không tính hai lần hoặc bỏ sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def generate(n, cur, arr):
    if n == 0:
        arr.append(int(cur))
        return
    generate(n - 1, cur + '6', arr)
    generate(n - 1, cur + '9', arr)

def main():
    arr = []
    for length in range(1, 19):
        generate(length, "", arr)

    arr.sort()

    def count_le(x):
        lo, hi = 0, len(arr)
        while lo < hi:
            mid = (lo + hi) // 2
            if arr[mid] <= x:
                lo = mid + 1
            else:
                hi = mid
        return lo

    t = int(input())
    for _ in range(t):
        l, r = map(int, input().split())
        print(count_le(r) - count_le(l - 1))

if __name__ == "__main__":
    main()
```Giải pháp bắt đầu bằng cách xây dựng tất cả các số hợp lệ bằng cách sử dụng trình tạo đệ quy xây dựng mọi độ dài từ 1 đến 18. Mỗi cấp độ đệ quy nối thêm chữ số 6 hoặc 9, đảm bảo bao phủ toàn bộ không gian quyết định nhị phân. Sau khi tất cả các số được thu thập, chúng sẽ được sắp xếp sao cho thứ tự số khớp với thứ tự mảng. 

chức năng`count_le(x)`thực hiện tìm kiếm nhị phân giới hạn trên tiêu chuẩn, trả về số lượng phần tử$\le x$. Câu trả lời cuối cùng cho mỗi truy vấn là sự khác biệt giữa số lượng tiền tố tại$r$Và$l-1$, cách ly chính xác khoảng thời gian. 

Một chi tiết quan trọng đang sử dụng`l - 1`thay vì cố gắng kiểm tra tư cách thành viên của`l`trực tiếp. Điều này tránh các trường hợp cạnh khi$l$chính nó là một số hợp lệ. 

## Ví dụ đã hoạt động 

Vì câu lệnh không bao gồm các mẫu rõ ràng nên hãy xem xét hai truy vấn minh họa. 

### Ví dụ 1 

Truy vấn đầu vào:$l = 6, r = 9$Chúng tôi tạo số tiền tố trên mảng được sắp xếp bắt đầu bằng các giá trị như 6, 9, 66, 69, ... 

| Bước | x | đếm_le(x) | 
| --- | --- | --- | 
| tiền tố ≤ 9 | 9 | 2 | 
| tiền tố 5 | 5 | 0 | 

Câu trả lời là$2 - 0 = 2$. 

Điều này xác nhận rằng cả hai số hợp lệ có một chữ số đều được đưa vào chính xác. 

### Ví dụ 2 

Truy vấn đầu vào:$l = 10, r = 69$Chúng tôi đếm tất cả các số hợp lệ đến 69. Chúng bao gồm 6, 9, 66, 69. 

| Bước | x | đếm_le(x) | 
| --- | --- | --- | 
| tiền tố ≤ 69 | 69 | 4 | 
| tiền tố ≤ 9 | 9 | 2 | 

Câu trả lời là$4 - 2 = 2$, tương ứng với 66 và 69. 

Điều này cho thấy các số có nhiều chữ số được đưa vào một cách tự nhiên sau khi thế hệ trải dài trên mọi chiều dài. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(2^{18} + q \log 2^{18})$| Tính toán trước tất cả các kết hợp chữ số một lần, sau đó tìm kiếm nhị phân cho mỗi truy vấn | 
| Không gian |$O(2^{18})$| Lưu trữ tất cả các số được tạo trong danh sách | 

Tổng số giá trị được tạo ra chỉ là 262.143, dễ dàng nằm gọn trong bộ nhớ và cho phép sắp xếp và truy vấn nhanh chóng. Ngay cả với số lượng lớn các trường hợp kiểm thử, mỗi truy vấn vẫn được xử lý theo thời gian logarit trên một mảng rất nhỏ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math
    from bisect import bisect_right

    arr = []
    def gen(n, s):
        if n == 0:
            arr.append(int(s))
            return
        gen(n - 1, s + "6")
        gen(n - 1, s + "9")

    for i in range(1, 5):
        gen(i, "")
    arr.sort()

    def count_le(x):
        return bisect_right(arr, x)

    it = iter(inp.strip().split())
    t = int(next(it))
    out = []
    for _ in range(t):
        l = int(next(it)); r = int(next(it))
        out.append(str(count_le(r) - count_le(l - 1)))
    return "\n".join(out)

# custom cases
assert run("1\n1 5\n") == "0"
assert run("1\n6 9\n") == "2"
assert run("1\n6 6\n") == "1"
assert run("1\n10 100\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1, 1 5 | 0 | không có số hợp lệ nào dưới đây nhỏ nhất | 
| 1, 6 9 | 2 | độ chính xác cơ sở một chữ số | 
| 1, 6 6 | 1 | bao gồm ranh giới chính xác | 
| 1, 10 100 | 2 | lọc nhiều chữ số | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi phạm vi truy vấn bắt đầu dưới số đẹp nhỏ nhất. Ví dụ,$l = 1, r = 5$. Thuật toán tính toán`count_le(5) - count_le(0)`. Vì không có giá trị được tạo ra$\le 5$, cả hai số đếm đều bằng 0, tạo ra kết quả đầu ra đúng 0. Phép trừ với$l-1$tự nhiên xử lý giới hạn dưới mà không cần phân nhánh đặc biệt. 

Một trường hợp khác là khi phạm vi khớp chính xác với số được tạo. Vì$l = r = 66$,`count_le(66)`bao gồm tất cả các giá trị lên đến và bao gồm 66, trong khi`count_le(65)`loại trừ nó. Sự khác biệt của chúng là 1, đếm chính xác số hợp lệ. 

Trường hợp tinh tế cuối cùng là sắp xếp: vì việc tạo không được sắp xếp vốn có, việc không sắp xếp sẽ phá vỡ tính chính xác của tìm kiếm nhị phân. Ví dụ: nếu 96 được tạo trước 66, số tiền tố sẽ trở nên vô nghĩa. Việc sắp xếp đảm bảo tính bất biến mà thứ tự số khớp với thứ tự mảng, điều này rất cần thiết cho tất cả logic truy vấn.
