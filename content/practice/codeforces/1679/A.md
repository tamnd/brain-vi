---
title: "CF 1679A - AvtoBus"
description: "Chúng tôi được yêu cầu xác định xem một đội xe buýt có thể có bao nhiêu chiếc xe buýt với tổng số bánh xe. Mỗi xe buýt có một trong hai loại: xe buýt hai trục với 4 bánh và xe buýt ba trục với 6 bánh."
date: "2026-06-10T00:40:07+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1679
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 791 (Div. 2)"
rating: 900
weight: 1679
solve_time_s: 97
verified: true
draft: false
---

[CF 1679A - AvtoBus](https://codeforces.com/problemset/problem/1679/A) 

**Xếp hạng:** 900 
**Tags:** sức mạnh vũ phu, lòng tham, toán học, lý thuyết số 
**Thời gian giải:** 1 phút 37s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu xác định xem một đội xe buýt có thể có bao nhiêu chiếc xe buýt với tổng số bánh xe. Mỗi xe buýt có một trong hai loại: xe buýt hai trục với 4 bánh và xe buýt ba trục với 6 bánh. Đầu vào bao gồm một số trường hợp thử nghiệm, mỗi trường hợp xác định tổng số bánh xe và đầu ra phải là số lượng xe buýt tối thiểu và tối đa có thể tính tổng bằng tổng số bánh xe này. Nếu không có sự kết hợp nào của xe buýt có thể tạo ra tổng số đó thì đầu ra là`-1`. 

Các ràng buộc rất rộng rãi đối với số học nhưng lại hạn chế đối với phép liệt kê đơn giản. Số lượng bánh xe`n`có thể lớn tới 10^18, nghĩa là bất kỳ giải pháp nào cố gắng liệt kê tất cả các tổ hợp xe buýt đều không khả thi. Giới hạn thời gian 1 giây với tối đa 1.000 ca kiểm thử ngụ ý rằng chúng ta cần giải pháp O(1) hoặc O(log n) cho mỗi ca kiểm thử. Các trường hợp cạnh bao gồm tổng số nhỏ như`1`,`2`, hoặc`3`, không thể hình thành bằng xe buýt 4 và 6 bánh, và các tổng rất lớn, phải được xử lý bằng số học số nguyên chính xác mà không bị tràn. 

Một cách tiếp cận đơn giản có thể thất bại với các tổng số lẻ hoặc không chia hết thành tổng 4 và 6. Ví dụ: không thể tạo thành 7 bánh xe vì 4+4=8 vượt quá nó và 6+4=10 cũng vượt quá nó. Đầu vào nhỏ như`4`hoặc`6`yêu cầu thuật toán để phát hiện rằng chỉ có thể có một bus duy nhất, đó là mức tối thiểu và tối đa đồng thời. 

## Phương pháp tiếp cận 

Phương pháp brute-force thử tất cả sự kết hợp của xe buýt hai trục và ba trục có tổng các bánh xe bằng`n`. Đối với mỗi`a`TRONG`[0, n//4]`, nó sẽ kiểm tra xem`n - 4*a`chia hết cho 6. Điều này đúng vì nó xem xét tất cả các phân vùng hợp lệ, nhưng nó yêu cầu tối đa n/4 lần lặp cho mỗi trường hợp thử nghiệm. Vì`n = 10^18`, điều này hoàn toàn không thể thực hiện được. 

Cái nhìn sâu sắc quan trọng đến từ số học số nguyên đơn giản. Cho phép`x`là số lượng xe buýt 2 trục và`y`số lượng xe buýt 3 trục. Khi đó ta có phương trình Diophantine tuyến tính`4*x + 6*y = n`. Chia cả hai vế cho 2 được`2*x + 3*y = n/2`. Bây giờ chúng ta có thể giải quyết cho`x`Và`y`dưới dạng số nguyên không âm. Số lượng xe buýt tối thiểu xảy ra khi chúng tôi tối đa hóa`y`, vì xe buýt 3 trục có nhiều bánh hơn trên mỗi xe buýt. Điều này dẫn đến`y = floor(n/6)`nếu như`n`chia hết cho 2 thì`x`là số bánh xe còn lại chia cho 4. Số lượng xe buýt tối đa xảy ra khi chúng ta tối đa hóa`x`, dẫn đến`x = floor(n/4)`Và`y`được điều chỉnh cho phù hợp. Nếu như`n`là số lẻ hoặc số dư sau khi chia cho 2 không chia hết cho 2 thì không tồn tại nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(1) | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. Đối với mỗi trường hợp thử nghiệm, hãy đọc tổng số bánh xe`n`. 
2. Kiểm tra xem`n`nhỏ hơn 4. Nếu vậy, xuất ra`-1`bởi vì không có tổ hợp xe buýt nào có thể có ít hơn 4 bánh. 
3. Tính số lượng xe buýt tối thiểu bằng cách lấy càng nhiều xe buýt 3 trục càng tốt. Bộ`min_buses = ceil(n/6)`vì mỗi xe 3 trục đóng góp 6 bánh. Nếu như`n`chia hết cho 2, số học số nguyên là đủ. 
4. Tính số lượng xe buýt tối đa bằng cách lấy càng nhiều xe buýt 2 trục càng tốt. Bộ`max_buses = n // 4`vì mỗi xe 2 trục đóng góp 4 bánh. 
5. Kiểm tra xem`n`chia hết cho 2. Nếu không, in`-1`vì 4 và 6 đều là số chẵn nên tổng số lẻ là không thể. Ngược lại, in`min_buses`Và`max_buses`. 

Tính đúng đắn dựa trên tính bất biến mà mọi sự kết hợp hợp lệ của xe buýt 4 và 6 bánh phải có tổng là một số chẵn. Việc tối đa hóa số lượng xe buýt 3 trục sẽ giảm thiểu tổng số lượng xe buýt và tối đa hóa số lượng xe buýt 2 trục sẽ tối đa hóa tổng số lượng xe buýt. Số học sàn và trần đảm bảo chúng ta không bỏ lỡ các nghiệm số nguyên khả thi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    if n < 4 or n % 2 != 0:
        print(-1)
        continue
    min_buses = (n + 5) // 6  # ceil(n / 6)
    max_buses = n // 4         # floor(n / 4)
    print(min_buses, max_buses)
```Mã đọc đầu vào hiệu quả bằng cách sử dụng`sys.stdin.readline`. Chúng tôi ngay lập tức lọc ra những tổng số quá nhỏ hoặc lẻ. Số lượng xe buýt tối thiểu sử dụng vách ngăn trần để đảm bảo có đủ bánh xe được phủ trên xe buýt 3 trục. Số lượng xe buýt tối đa sử dụng cách chia sàn để tối đa hóa xe buýt 2 trục. Cả hai cách chia đều hoạt động chính xác với số nguyên rất lớn trong Python. 

## Ví dụ đã hoạt động 

Đối với đầu vào`n = 4`,`(4+5)//6 = 1`cho xe buýt tối thiểu và`4//4 = 1`tối đa. Đầu ra là`1 1`, phù hợp với mong đợi. Vì`n = 24`,`(24+5)//6 = 29//6 = 4`tối thiểu và`24//4 = 6`tối đa, sản xuất`4 6`, phù hợp với lời giải thích mẫu. 

| n | min_bus | max_buses | 
| --- | --- | --- | 
| 4 | 1 | 1 | 
| 24 | 4 | 6 | 
| 7 | - | - | 
| 998244353998244352 | 166374058999707392 | 249561088499561088 | 

Những dấu vết này xác nhận rằng thuật toán xử lý các đầu vào nhỏ, trung bình và rất lớn và loại bỏ chính xác các trường hợp không thể xảy ra như 7. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Một phép tính số học cho mỗi trường hợp thử nghiệm | 
| Không gian | O(1) | Chỉ một vài biến số nguyên được sử dụng cho mỗi trường hợp thử nghiệm | 

Ngay cả với t = 1.000 và n lên đến 10^18, các thao tác O(1) này cho mỗi trường hợp kiểm thử vẫn phù hợp trong giới hạn 1 giây. Python xử lý các số nguyên lớn tùy ý một cách hiệu quả cho các phép tính này. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    t = int(input())
    for _ in range(t):
        n = int(input())
        if n < 4 or n % 2 != 0:
            print(-1)
            continue
        min_buses = (n + 5) // 6
        max_buses = n // 4
        print(min_buses, max_buses)
    return out.getvalue().strip()

# provided samples
assert run("4\n4\n7\n24\n998244353998244352\n") == "1 1\n-1\n4 6\n166374058999707392 249561088499561088"

# custom cases
assert run("3\n1\n2\n3\n") == "-1\n-1\n-1", "too small numbers"
assert run("2\n6\n10\n") == "1 1\n2 2", "small valid numbers"
assert run("1\n1000000000000000000\n") == "166666666666666667 250000000000000000", "very large number"
assert run("1\n5\n") == "-1", "odd number less than 6"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1,2,3 | -1,-1,-1 | từ chối tổng số nhỏ không thể | 
| 6,10 | 1 1, 2 2 | số hợp lệ nhỏ | 
| 10^18 | 166666666666666667 250000000000000000 | độ chính xác của số rất lớn | 
| 5 | -1 | phát hiện số lẻ | 

## Vỏ cạnh 

Khi nào`n`nhỏ hơn 4 thì thuật toán trả về ngay`-1`. Đối với các số lẻ như`7`,`5`, hoặc`1`, thuật toán trả về`-1`vì 4 và 6 không thể tổng thành số lẻ. Đối với bội số của 2 nhưng không phải bội số của 4 hoặc 6, chẳng hạn như`10`, thuật toán tính toán chính xác mức tối thiểu và tối đa bằng cách phân phối phần còn lại một cách thích hợp. Đối với các số cực lớn, Python xử lý số học số nguyên mà không bị tràn, đảm bảo tính chính xác ngay cả khi`n`tiến tới 10^18.
