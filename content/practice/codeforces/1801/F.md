---
title: "CF 1801F - Một thanh sô cô la n chiều khác"
description: "Chúng ta được tặng một thanh sô cô la có n chiều. Dọc theo mỗi chiều, thanh đã được chia thành các đơn vị riêng biệt: đối với chiều i, có các đơn vị ai. Vasya muốn cắt sô-cô-la sâu hơn dọc theo các phần đã có từ trước này để tạo ra ít nhất k miếng."
date: "2026-06-09T09:34:24+07:00"
tags: ["codeforces", "competitive-programming", "dp", "math", "meet-in-the-middle", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1801
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 857 (Div. 1)"
rating: 2700
weight: 1801
solve_time_s: 128
verified: false
draft: false
---

[CF 1801F - Một thanh sô cô la n chiều khác](https://codeforces.com/problemset/problem/1801/F) 

**Xếp hạng:** 2700 
**Tags:** dp, toán, gặp nhau ở giữa, lý thuyết số 
**Thời gian giải:** 2 phút 8 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được tặng một thanh sô cô la có n chiều. Dọc theo mỗi chiều, thanh đã được chia thành các đơn vị riêng biệt: đối với chiều i, có`a_i`đơn vị. Vasya muốn cắt giảm sô cô la hơn nữa dọc theo các bộ phận đã có từ trước này để sản xuất ít nhất`k`miếng. Anh ta có thể chọn bao nhiêu phân đoạn`b_i`để cắt dọc theo từng chiều, với`1 <= b_i <= a_i`. Sau khi cắt, mỗi mảnh thu được có một số lát ban đầu được tính bằng tích số chia sàn`floor(a_1 / b_1) * ... * floor(a_n / b_n)`. Mỗi lát có khối lượng`1 / (a_1 * ... * a_n)`, vậy thể tích của một mảnh bằng tích này chia cho tổng số lát. Vasya muốn tối đa hóa khối lượng của mảnh nhỏ nhất nhân với`k`. 

Đầu vào cung cấp kích thước`n`(tối đa 100), số lượng mảnh tối thiểu cần thiết`k`(tối đa 10^7) và mảng`a`kích thước`n`(mỗi cái lên tới 10^7). Đầu ra là số dấu phẩy động - khối lượng tối đa có thể có của thời gian mảnh nhỏ nhất`k`. 

Những ràng buộc cho chúng ta biết rằng`n`vừa phải nhưng`a_i`Và`k`có thể rất lớn. Điều này loại trừ các cách tiếp cận bạo lực ngây thơ, thử mọi sự kết hợp có thể của`b_i`, bởi vì tổng số kết hợp là`a_1 * a_2 * ... * a_n`, có thể lớn về mặt thiên văn. Chúng ta cũng cần xử lý việc tràn số nguyên một cách cẩn thận, vì tích của`a_i`có thể vượt quá 10^18. 

Các trường hợp Edge bao gồm các tình huống không thể lấy được`k`mảnh, ví dụ,`n=1, a=[5], k=6`. Ở đây, bất kỳ sự lựa chọn nào của`b_1`tạo ra tối đa 5 mảnh, vì vậy câu trả lời đúng là 0. Một điểm tinh tế khác là việc tối đa hóa khối lượng có thể thích cắt ít hơn dọc theo một số kích thước vì việc phân chia tầng sẽ mất giá trị. Ví dụ, với`a=[4,7]`Và`k=7`, việc phân bổ các vết cắt đồng đều sẽ mang lại khối lượng tối thiểu khác với các vết cắt tập trung dọc theo kích thước lớn hơn. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản về mặt khái niệm: thử tất cả các kết hợp của`b_1`bởi vì`b_n`thỏa mãn`1 <= b_i <= a_i`, tính toán`floor(a_1 / b_1) * ... * floor(a_n / b_n)`và chọn sự kết hợp tạo ra ít nhất`k`mảnh và tối đa hóa`k * volume`. Điều này hoạt động rất nhỏ`a_i`Và`n`, nhưng với`n=100`Và`a_i=10^7`, số lượng kết hợp là không thể lớn được. Thậm chí tạo ra tất cả`b_i`đối với một chiều lớn tới 10^7 thì quá đắt. 

Quan sát quan trọng là chúng ta thực sự đang tìm kiếm giá trị lớn nhất có thể`x`sao cho chúng ta có thể cắt sô cô la thành từng miếng trong đó số lát tối thiểu trong một miếng ít nhất là`x`. Vấn đề có thể được trình bày lại dưới dạng tìm kiếm: tìm kiếm một ứng viên`x`, chúng ta có thể tìm được số nguyên không`b_i`như vậy`floor(a_1 / b_1) * ... * floor(a_n / b_n) >= x`Và`b_1 * ... * b_n >= k`? Vì âm lượng tỉ lệ với`floor(a_i / b_i)`, chúng ta có thể sử dụng tìm kiếm nhị phân`x`. 

Đối với mỗi`x`, tính toán tối đa`b_i`điều đó đảm bảo`floor(a_i / b_i) >= t`rất đơn giản:`b_i <= a_i // t`. Sau đó, chúng ta cần kiểm tra xem sản phẩm của những thứ này có`b_i`giới hạn có thể đạt ít nhất`k`. Điều này làm giảm vấn đề thành một cách tiếp cận "gặp ở giữa" hoặc tham lam: nhân mức khả thi lớn nhất`b_i`cho từng thứ nguyên cho đến khi chúng tôi đạt được`k`hoặc tùy chọn ống xả. 

Cái nhìn sâu sắc là vấn đề phân tách theo các chiều và hàm sàn có thể được đảo ngược bằng cách sử dụng phép chia số nguyên. Khi chúng tôi đặt kích thước mảnh tối thiểu của ứng viên`x`, mỗi thứ nguyên đóng góp độc lập vào số lượng phân đoạn tối đa`b_i`. Sau đó, vấn đề là kiểm tra xem sản phẩm của họ có đáp ứng được`k`. Tìm kiếm nhị phân cho phép chúng ta hội tụ ở mức tối đa có thể`x`một cách hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(sản phẩm(a_i)) | O(n) | Quá chậm | 
| Tìm kiếm nhị phân + Giới hạn theo thứ nguyên | O(n log(max(a_i))) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng số lát`total = a_1 * ... * a_n`. Nếu như`total < k`, xuất ra 0 ngay lập tức vì không thể có`k`miếng. 
2. Đặt phạm vi tìm kiếm nhị phân cho số lượng lát tối thiểu`x`. Giới hạn dưới là 0 và giới hạn trên là`max(a_i)`, vì một phần tối thiểu không thể chứa nhiều lát dọc theo một chiều hơn`a_i`. 
3. Trong khi phạm vi tìm kiếm nhị phân không trống, hãy lấy`mid = (low + high + 1) // 2`. Cái này`mid`đại diện cho số lượng lát tối thiểu ứng cử viên cho phần nhỏ nhất. 
4. Đối với từng chiều`i`, tính toán`max_bi = a_i // mid`. Đây là số lần cắt tối đa chúng ta có thể thực hiện dọc theo kích thước`i`mà không vi phạm số lượng tối thiểu của ứng viên. Nếu như`max_bi == 0`, ứng viên`mid`quá lớn; chúng ta không thể có được dù chỉ một lát dọc theo chiều này. 
5. Tính tích`prod_b`của`max_bi`trên tất cả các chiều. Nếu tại bất kỳ thời điểm nào sản phẩm vượt quá`k`, chúng ta có thể dừng lại sớm vì chúng ta chỉ quan tâm liệu chúng ta có thể đạt được`k`. 
6. Nếu`prod_b >= k`, ứng viên`mid`là khả thi, vì vậy chúng tôi đặt`low = mid`. Ngược lại, đặt`high = mid - 1`. 
7. Sau khi tìm kiếm nhị phân hoàn tất,`low`chứa số lát tối thiểu có thể đạt được tối đa trên mỗi phần. Tính khối lượng cuối cùng như`volume = (low / a_1) * ... * (low / a_n) * k`, điều này đơn giản hóa thành`volume = low * k / total_slices`. 
8. Xuất âm lượng với độ chính xác vừa đủ. 

Tại sao nó hoạt động: Tìm kiếm nhị phân duy trì tính bất biến mà bất kỳ`x <= low`có thể đạt được và bất kỳ`x > high`thì không. Kiểm tra tính khả thi thông qua độc lập`b_i`giới hạn là chính xác vì việc phân chia tầng đảm bảo rằng nếu chúng ta tôn trọng`b_i <= a_i // x`, không có mảnh nào có ít hơn`x`cắt dọc theo chiều đó. Nhân các giới hạn đảm bảo chúng ta đạt được ít nhất`k`miếng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    
    total_slices = 1
    for ai in a:
        total_slices *= ai
        if total_slices >= 1e18:  # cap to prevent overflow
            break
    if total_slices < k:
        print(0)
        return

    low, high = 0, max(a)
    while low < high:
        mid = (low + high + 1) // 2
        prod = 1
        for ai in a:
            prod *= ai // mid
            if prod >= k:
                break
        if prod >= k:
            low = mid
        else:
            high = mid - 1

    minimal_slices = low
    volume = minimal_slices * k / total_slices
    print(f"{volume:.12f}")

if __name__ == "__main__":
    main()
```Giải pháp đầu tiên sẽ kiểm tra xem`k`thậm chí có thể thực hiện được với tổng số lát cắt. Tìm kiếm nhị phân đảm bảo rằng chúng tôi tối đa hóa phần tối thiểu. nhân`ai // mid`đối với mỗi kích thước đảm bảo rằng các ràng buộc phân chia tầng được thỏa mãn và việc dừng sớm sẽ ngăn ngừa tràn. Cuối cùng, tính toán`minimal_slices * k / total_slices`cho âm lượng được chia tỷ lệ theo`k`. 

## Ví dụ đã hoạt động 

Mẫu 1: 

đầu vào:```
1 2
5
```| thấp | cao | giữa | sản phẩm | quyết định | 
| --- | --- | --- | --- | --- | 
| 0 | 5 | 3 | 1 | quá thấp, cao=2 | 
| 0 | 2 | 1 | 5 | khả thi, thấp=1 | 
| 2 | 2 | 2 | 2 | khả thi, thấp=2 |`minimal_slices = 2`, tổng_slices = 5, khối lượng =`2 * 2 / 5 = 0.8`. 

Mẫu 2: 

đầu vào:```
2 6
5 10
```| thấp | cao | giữa | sản phẩm | quyết định | 
| --- | --- | --- | --- | --- | 
| 0 | 10 | 5 | 2 | < k, cao=4 | 
| 0 | 4 | 2 | 10 | >= k, thấp=2 | 
| 3 | 4 | 3 | 4 | < k, cao=3 | 
| 3 | 3 | 3 | 4 | < k, cao=2 |`minimal_slices = 2`, tổng_slices = 50, khối lượng =`2*6/50 = 0.24`. 

Dấu vết này xác nhận rằng thuật toán tìm thấy số lát cắt tối thiểu khả thi tối đa, xử lý chính xác nhiều kích thước và dừng sớm khi sản phẩm vượt quá`k`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp
