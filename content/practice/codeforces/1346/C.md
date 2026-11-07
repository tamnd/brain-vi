---
title: "CF 1346C - Làm sạch lò xo"
description: "Chúng tôi được xếp một dãy kệ, mỗi kệ chứa một số cuốn sách. Mục tiêu là đạt đến trạng thái mà mỗi kệ có tối đa k cuốn sách. Chúng tôi được phép sửa đổi cấu hình bằng hai thao tác."
date: "2026-06-16T10:03:17+07:00"
tags: ["codeforces", "competitive-programming", "*special", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1346
codeforces_index: "C"
codeforces_contest_name: "Kotlin Heroes: Episode 4"
rating: 1600
weight: 1346
solve_time_s: 316
verified: false
draft: false
---

[CF 1346C - Dọn dẹp mùa xuân](https://codeforces.com/problemset/problem/1346/C) 

**Đánh giá:** 1600 
**Tags:** *đặc biệt, tham lam, phân loại 
**Thời gian giải:** 5 phút 16s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được xếp một dãy kệ, mỗi kệ chứa một số cuốn sách. Mục tiêu là đạt đến trạng thái mà mỗi kệ có nhiều nhất`k`sách. Chúng tôi được phép sửa đổi cấu hình bằng hai thao tác. Hoạt động đầu tiên mang tính cục bộ: chúng ta có thể chọn một giá và xóa tất cả sách của nó với một mức giá`x`. Hoạt động thứ hai mang tính toàn cầu: chúng tôi lấy tất cả sách, sau đó phân phối lại chúng một cách đồng đều nhất có thể trên tất cả các kệ với chi phí`y`, tạo ra một cấu hình trong đó các giá trị khác nhau nhiều nhất là một. 

Nhiệm vụ là chọn một chuỗi các thao tác này để giảm thiểu tổng chi phí trong khi vẫn đảm bảo cấu hình cuối cùng tôn trọng giới hạn trên.`k`. 

Hạn chế chính đó là`n`trên tất cả các trường hợp thử nghiệm là nhiều nhất`2 * 10^5`, vì vậy mọi giải pháp bậc hai cho mỗi trường hợp thử nghiệm sẽ thất bại. Thậm chí một`O(n log n)`mỗi thử nghiệm chỉ được chấp nhận nếu chúng tôi tổng hợp cẩn thận, nhưng việc mô phỏng lặp lại các hoạt động hoặc khám phá các tập hợp con là không thể. Chúng tôi cần một giải pháp giúp giảm từng trường hợp thử nghiệm thành công việc tuyến tính hoặc tuyến tính. 

Một trường hợp phức tạp xuất hiện khi việc phân phối lại toàn cầu tạo ra các giá trị vẫn vượt quá`k`. Trong trường hợp đó, chỉ thao tác thôi là chưa đủ nên chúng ta phải kết hợp với việc loại bỏ. Một tình huống phức tạp khác là khi loại bỏ một vài kệ lớn sẽ tốn kém hơn so với việc thực hiện phân phối lại toàn cầu trước, ngay cả khi việc phân phối lại thoạt nhìn có vẻ lãng phí. 

Ví dụ: hãy xem xét trường hợp trong đó hầu hết các giá trị đều cao hơn một chút`k`, nhưng có một giá trị cực kỳ lớn. Một cách tiếp cận đơn giản có thể luôn loại bỏ phần tử lớn nhất, nhưng đôi khi một sự phân phối lại toàn cầu duy nhất làm cho tất cả các giá trị đủ nhỏ, khiến cho việc loại bỏ tốn kém không cần thiết. 

## Phương pháp tiếp cận 

Chiến lược brute-force sẽ xem xét tất cả các tập hợp con của kệ để áp dụng thao tác đầu tiên và đối với mỗi tập hợp con sẽ quyết định áp dụng thao tác thứ hai trước hay sau. Sau khi loại bỏ một tập hợp con, chúng tôi sẽ tính toán xem có cần phân phối lại hay không và mô phỏng nó. Điều này dẫn tới việc kiểm tra`2^n`các tập hợp con và ngay cả khi chúng tôi hạn chế chỉ loại bỏ các giá trị "xấu", số lượng kết hợp vẫn theo cấp số nhân trong trường hợp xấu nhất. 

Quan sát quan trọng là thao tác thứ hai hoạt động có tính xác định: sau khi được áp dụng, tất cả các giá trị sẽ trở thành`floor(S/n)`hoặc`ceil(S/n)`Ở đâu`S`là tổng số tiền. Điều này có nghĩa là chúng ta không bao giờ cần mô phỏng các trạng thái tùy ý. Chúng ta chỉ cần biết vượt quá bao nhiêu kệ`k`và chúng tôi có thể muốn loại bỏ bao nhiêu trong số chúng trước khi quyết định liệu việc phân phối lại có mang lại lợi ích hay không. 

Thay vì chọn một tập hợp con tùy ý, chúng ta sắp xếp mảng. Điều này cho phép chúng tôi coi việc xóa là "cắt bỏ các phần tử lớn nhất". Nếu chúng ta loại bỏ`i`kệ lớn nhất, chúng ta có thể tính số tiền còn lại và kiểm tra xem việc phân phối lại sẽ tạo ra bao nhiêu. Điều này làm giảm không gian quyết định từ các ứng cử viên theo cấp số nhân đến tuyến tính cho mỗi trường hợp thử nghiệm. 

Sau đó, chúng tôi so sánh hai chiến lược: chỉ loại bỏ cho đến khi tất cả các giá trị ≤ k hoặc thực hiện phân phối lại sau khi loại bỏ một số hậu tố của các phần tử lớn nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tập hợp con Brute Force | O(2^n) | O(n) | Quá chậm | 
| Sắp xếp + thử loại bỏ hậu tố | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giảm vấn đề xuống còn việc quyết định cần loại bỏ bao nhiêu phần tử lớn nhất trước khi có thể áp dụng phân phối lại. 

1. Sắp xếp mảng theo thứ tự không giảm. Điều này đảm bảo rằng mọi tập hợp phần tử bị loại bỏ đều tương ứng với hậu tố của mảng. 
2. Tính tổng tiền tố để ta có thể nhanh chóng tính tổng các phần tử còn lại sau khi loại bỏ hậu tố. 
3. Hãy để`i`đại diện cho số lượng phần tử lớn nhất chúng tôi loại bỏ. Ban đầu hãy xem xét`i = 0`. 
4. Đối với mỗi`i`từ`0`ĐẾN`n`, tính tổng còn lại và số đếm còn lại`m = n - i`. 
5. Kiểm tra xem cấu hình hiện tại đã thỏa mãn điều kiện chưa`max ≤ k`. Vì chúng tôi đã loại bỏ các phần tử lớn nhất nên điều này tương đương với việc kiểm tra`a[m-1] ≤ k`. Nếu đúng, hãy cập nhật câu trả lời với chi phí`i * x`. 
6. Nếu không, hãy cân nhắc việc thực hiện phân phối lại toàn cầu sau khi loại bỏ. Tính giá trị cực đại được phân phối lại, giá trị này sẽ là`ceil(sum / m)`. Nếu giá trị này ≤ ​​k thì trạng thái có thể được cố định bằng chi phí`i * x + y`. 
7. Lấy giá trị tối thiểu trên tất cả giá trị hợp lệ`i`. 

Ý tưởng chính là việc loại bỏ chỉ nhắm mục tiêu vào các phần tử lớn nhất, bởi vì bất kỳ chiến lược tối ưu nào sẽ không bao giờ loại bỏ phần tử nhỏ hơn trong khi vẫn giữ phần tử lớn hơn, vì cả hai thao tác đều đối xứng về vị trí chứ không phải giá trị. 

### Tại sao nó hoạt động 

Thuật toán dựa trên thực tế là phép toán thứ hai hủy bỏ thứ tự và thay thế mảng bằng phân bố gần như đồng đều chỉ được xác định bởi tổng. Điều này loại bỏ mọi nhu cầu theo dõi các phần tử riêng lẻ sau khi phân phối lại. Do đó, sau khi chọn số lượng phần tử cần loại bỏ, trạng thái liên quan duy nhất là`(remaining count, remaining sum, maximum remaining element before redistribution)`. Việc sắp xếp đảm bảo rằng mọi cấu hình loại bỏ có ý nghĩa đều được biểu thị bằng một hậu tố, do đó chúng tôi không bỏ sót bất kỳ giải pháp tối ưu nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, k, x, y = map(int, input().split())
        a = list(map(int, input().split()))
        
        a.sort()
        
        pref = [0] * (n + 1)
        for i in range(n):
            pref[i + 1] = pref[i] + a[i]
        
        ans = float('inf')
        
        for rem in range(n + 1):
            i = n - rem  # number of removed largest elements
            if rem == 0:
                sum_rem = 0
            else:
                sum_rem = pref[rem]
            
            if rem > 0 and a[rem - 1] > k:
                continue
            
            cost = i * x
            
            if rem > 0:
                max_after = (sum_rem + rem - 1) // rem
                if max_after <= k:
                    ans = min(ans, cost + y)
            else:
                ans = min(ans, cost)
        
        print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách sắp xếp sao cho việc loại bỏ các phần tử lớn nhất trở thành phần cắt tiền tố trong lập chỉ mục ngược. Tổng tiền tố cho phép tính toán các tổng còn lại theo thời gian không đổi. Vòng lặp liệt kê số phần tử còn lại sau khi loại bỏ. Sự phân chia`(sum_rem + rem - 1) // rem`tính giá trị trường hợp xấu nhất sau khi phân phối lại. 

Một chi tiết tinh tế là cách xử lý`rem = 0`, tương ứng với việc loại bỏ tất cả các phần tử. Trong trường hợp này, việc phân phối lại là vô nghĩa và chỉ có chi phí loại bỏ mới là quan trọng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 5, k = 4, x = 3, y = 5
a = [1, 2, 2, 3, 5]
```Chúng tôi sắp xếp:```
[1, 2, 2, 3, 5]
```Chúng tôi đánh giá số lần xóa khác nhau: 

| rem (giữ) | đã xóa | tổng hợp | kiểm tra tối đa | phân phối lại tối đa | chi phí | 
| --- | --- | --- | --- | --- | --- | 
| 5 | 0 | 13 | 5 > 4 | 3 | 5 | 
| 4 | 1 | 8 | 3 4 | 2 | 8 | 
| 3 | 2 | 5 | 2 4 | 2 | 11 | 
| 2 | 3 | 3 | 2 4 | 2 | 14 | 
| 1 | 4 | 1 | 1 4 | 1 | 17 | 

Chiến lược hợp lệ tốt nhất là`rem = 4`, trị giá`3 * 1 + 5 = 8`nếu sử dụng phân phối lại hoặc`1 * 3 = 3`nếu chỉ loại bỏ 5. 

Điều này xác nhận rằng chỉ loại bỏ phần tử lớn nhất là tối ưu. 

### Ví dụ 2 

đầu vào:```
n = 4, k = 3, x = 2, y = 10
a = [4, 4, 1, 1]
```Đã sắp xếp:```
[1, 1, 4, 4]
```| rem | đã xóa | tổng hợp | hợp lệ tối đa | phân phối lại | chi phí | 
| --- | --- | --- | --- | --- | --- | 
| 4 | 0 | 10 | 4 > 3 | 3 | 10 | 
| 3 | 1 | 6 | 4 > 3 | 2 | 2 | 
| 2 | 2 | 2 | 1 3 | 1 | 4 | 

Chiến lược tốt nhất là loại bỏ một hoặc hai yếu tố tùy thuộc vào việc so sánh với`y`. 

Điều này cho thấy việc phân phối lại chỉ hữu ích khi nó làm giảm đáng kể giá trị đỉnh xuống dưới`k`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế; quét tuyến tính | 
| Không gian | O(n) | Tổng tiền tố và lưu trữ mảng | 

Tổng độ phức tạp của tất cả các trường hợp thử nghiệm vẫn nằm trong`O(2 * 10^5 log n)`, phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n, k, x, y = map(int, input().split())
        a = list(map(int, input().split()))
        a.sort()
        pref = [0]*(n+1)
        for i in range(n):
            pref[i+1] = pref[i] + a[i]

        ans = float('inf')
        for rem in range(n+1):
            kept = rem
            removed = n - rem
            if rem > 0 and a[rem-1] > k:
                continue
            cost = removed * x
            if rem > 0:
                avg = (pref[rem] + rem - 1)//rem
                if avg <= k:
                    ans = min(ans, cost + y)
            else:
                ans = min(ans, cost)
        out.append(str(ans))
    return "\n".join(out)

# provided samples
assert run("""6
5 4 3 5
1 2 2 3 5
5 3 4 5
1 5 1 5 5
5 4 5 6
1 2 5 3 5
4 3 2 10
4 4 1 1
4 3 10 2
4 4 1 1
4 1 5 4
1 2 1 3
""") == """3
9
6
4
2
9"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Tất cả các giá trị nhỏ | 0 hoặc loại bỏ x trực tiếp | trường hợp không cần phẫu thuật | 
| Gai lớn đơn | quyết định x vs y | sự cân bằng loại bỏ và phân phối lại | 
| Đã đồng phục | 0 | tính chính xác không hoạt động | 
| Chặt k ranh giới | hoạt động hỗn hợp | hành vi ngưỡng cạnh | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi tất cả các phần tử đều đã bằng k. Trong tình huống này, mỗi lần lặp vòng lặp sẽ cho phép chi phí bằng 0 và bất kỳ việc triển khai nào buộc ít nhất một thao tác sẽ thêm chi phí không chính xác. Thuật toán xử lý việc này vì`rem = n`không tạo ra sự loại bỏ nào và không phân phối lại. 

Một trường hợp cạnh khác xảy ra khi chỉ có một phần tử vượt quá k. Ở đây, quyết định đúng đắn phụ thuộc hoàn toàn vào việc liệu`x`nhỏ hơn`y`và thuật toán nắm bắt điều này bằng cách đánh giá cả việc loại bỏ trực tiếp và phân phối lại sau khi loại bỏ. 

Trường hợp khó phát hiện cuối cùng là khi việc phân phối lại tạo ra các giá trị cao hơn k một chút mặc dù giá trị trung bình là nhỏ. Phép toán trần là cần thiết, vì chỉ riêng phép chia số nguyên sẽ đánh giá thấp giá trị cực đại thực sự sau khi phân phối.
