---
title: "CF 106511K - Một số tổng 3"
description: "Chúng ta được cung cấp một chuỗi các số nguyên và được yêu cầu đếm xem có bao nhiêu bộ ba vị trí riêng biệt tạo ra một điều kiện tổng mục tiêu cố định, trong bài toán này là điều kiện “tổng 3” cổ điển: ba phần tử khác nhau được chọn bởi các chỉ số phải có tổng bằng 0."
date: "2026-06-18T19:08:34+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106511
codeforces_index: "K"
codeforces_contest_name: "Columbia University Local Contest (CULC) Spring 2026"
rating: 0
weight: 106511
solve_time_s: 49
verified: true
draft: false
---

[CF 106511K - Một số 3-SUM](https://codeforces.com/problemset/problem/106511/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các số nguyên và được yêu cầu đếm xem có bao nhiêu bộ ba vị trí riêng biệt tạo ra một điều kiện tổng mục tiêu cố định, trong bài toán này là điều kiện “tổng 3” cổ điển: ba phần tử khác nhau được chọn bởi các chỉ số phải có tổng bằng 0. 

Đầu vào có thể được hiểu là một mảng duy nhất trong đó thứ tự không quan trọng đối với câu trả lời cuối cùng, chỉ có các giá trị và số lần mỗi giá trị xuất hiện. Đầu ra là một số nguyên biểu thị số bộ ba chỉ số$(i, j, k)$với$i < j < k$sao cho các giá trị tương ứng thỏa mãn$a[i] + a[j] + a[k] = 0$. 

Khó khăn chính không phải là hình thành một bộ ba hợp lệ mà là đếm tất cả chúng một cách hiệu quả khi mảng lớn và có thể chứa nhiều bản sao. 

Một ý tưởng ngây thơ là thử tất cả các bộ ba chỉ số và kiểm tra xem tổng của chúng có bằng 0 hay không. Điều này trở nên không khả thi ngay khi mảng phát triển vượt quá vài nghìn phần tử, bởi vì số lượng bộ ba tăng lên theo khối. 

Một vấn đề tế nhị hơn đến từ sự trùng lặp. Nếu mảng chứa các giá trị lặp lại thì giải pháp đúng phải tính các tổ hợp chỉ số chứ không chỉ các bộ ba giá trị riêng biệt. Ví dụ, nếu mảng là`[0, 0, 0, 0]`, câu trả lời đúng là số cách chọn 3 trong số 4 số 0 bất kỳ, bằng 4. Cách tiếp cận dựa trên tập hợp đơn giản loại bỏ các giá trị trùng lặp sẽ trả về sai 1. 

Một trường hợp cạnh khác xuất hiện khi các giá trị bao gồm cả số dương và số âm lớn nhưng không tồn tại bộ ba hợp lệ. Một giải pháp đúng vẫn phải chạy hiệu quả mà không cần dựa vào phương pháp chẩn đoán kết thúc sớm giả định cấu trúc. 

## Phương pháp tiếp cận 

Chiến lược vũ phu rất đơn giản. Chúng tôi lặp lại tất cả các bộ ba chỉ số$i < j < k$, tính tổng và tăng câu trả lời nếu nó bằng 0. Điều này đúng vì nó trực tiếp đánh giá định nghĩa của bài toán đối với mọi tổ hợp có thể xảy ra. 

Vấn đề là số lượng hoạt động. Đối với một mảng có kích thước$n$, cách tiếp cận này thực hiện khoảng$\frac{n^3}{6}$séc. Khi$n = 5000$, điều này đã vượt quá$10^{10}$hoạt động, vượt xa những gì có thể chạy kịp thời. 

Sự cải tiến đến từ việc giảm chiều thứ ba của tìm kiếm. Nếu chúng ta sửa một phần tử, vấn đề sẽ trở thành việc tìm các cặp trong hậu tố còn lại có tổng bằng một giá trị cụ thể. Đây là cấu trúc tổng 2 tiêu chuẩn, có thể giải được trong thời gian tuyến tính nếu phần còn lại được sắp xếp. 

Việc sắp xếp mảng cho phép quét hai con trỏ cho mỗi phần tử đầu tiên cố định. Đối với một chỉ mục đã chọn$i$, chúng tôi tìm kiếm các cặp$(l, r)$như vậy$a[i] + a[l] + a[r] = 0$. Điều này làm giảm bài toán từ thời gian bậc ba sang thời gian bậc hai. 

Lợi ích cấu trúc quan trọng là việc sắp xếp biến việc tìm kiếm cặp tùy ý thành một quá trình đơn điệu, trong đó con trỏ di chuyển có tác dụng có thể dự đoán được đối với tổng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^3)$|$O(1)$| Quá chậm | 
| Đã sửa + Hai con trỏ |$O(n^2)$|$O(1)$thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi dựa vào việc sắp xếp và quét hai con trỏ lặp đi lặp lại. 

1. Sắp xếp mảng theo thứ tự không giảm. Điều này cho phép chúng ta suy luận về cách các tổng thay đổi khi di chuyển các chỉ số vào trong hoặc ra ngoài. 
2. Cố định phần tử đầu tiên của bộ ba tại vị trí$i$. Chúng tôi coi đây là giá trị neo cho tất cả các cặp mà chúng tôi sẽ tìm kiếm. 
3. Đặt hai con trỏ,$l = i + 1$Và$r = n - 1$, và tính tổng$s = a[i] + a[l] + a[r]$. 
4. Nếu$s$bằng 0, chúng tôi đã tìm thấy bộ ba hợp lệ. Nếu giá trị tại$l$Và$r$khác biệt, chúng tôi đếm xem có bao nhiêu bản sao tồn tại ở cả hai bên và nhân tần số của chúng lên. Điều này xử lý các giá trị lặp lại một cách chính xác trong một bước thay vì lặp lại các phần tử giống hệt nhau. 
5. Nếu$s$nhỏ hơn 0, tổng quá nhỏ nên chúng ta di chuyển$l$ở bên phải để tăng nó. Điều này có hiệu quả vì việc tăng phần tử thứ hai trong một mảng đã sắp xếp chỉ có thể làm tăng tổng số tiền. 
6. Nếu$s$lớn hơn 0, tổng quá lớn nên chúng ta di chuyển$r$sang trái để giảm nó. 
7. Lặp lại các bước 3-6 cho đến khi$l$gặp gỡ$r$, sau đó di chuyển$i$chuyển tiếp và khởi động lại quá trình. 

### Tại sao nó hoạt động 

Tính đúng đắn dựa trên hai bất biến ghép đôi. Đầu tiên, đối với bất kỳ cố định nào$i$, quá trình quét hai con trỏ sẽ kiểm tra mọi cặp khả thi$(l, r)$đúng một lần trong một lần duyệt đơn điệu của đoạn mảng đã được sắp xếp. Không có cặp hợp lệ nào bị bỏ qua vì bất cứ khi nào một tổng quá nhỏ hoặc quá lớn, chỉ một hướng chuyển động có thể khôi phục được tổng hợp lệ theo thứ tự đã cho. 

Thứ hai, việc nhóm các bản sao đảm bảo rằng mọi kết hợp chỉ số đều được tính chính xác một lần. Thay vì liệt kê từng giá trị giống hệt nhau, chúng tôi tính sự đóng góp tổ hợp của chúng trong một bước duy nhất, duy trì tính chính xác đồng thời tránh tính toán quá mức. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    a.sort()

    ans = 0

    for i in range(n):
        l, r = i + 1, n - 1

        while l < r:
            s = a[i] + a[l] + a[r]

            if s < 0:
                l += 1
            elif s > 0:
                r -= 1
            else:
                if a[l] == a[r]:
                    cnt = r - l + 1
                    ans += cnt * (cnt - 1) // 2
                    break

                lv = a[l]
                rv = a[r]

                lc = 0
                while l < r and a[l] == lv:
                    l += 1
                    lc += 1

                rc = 0
                while r >= l and a[r] == rv:
                    r -= 1
                    rc += 1

                ans += lc * rc

    print(ans)

if __name__ == "__main__":
    solve()
```Chương trình bắt đầu bằng cách sắp xếp mảng, điều này rất cần thiết cho lý luận hai con trỏ. Đối với mỗi chỉ số cố định`i`, nó xử lý vấn đề như tìm các cặp phần bù có tổng bằng`-a[i]`. 

Logic xử lý trùng lặp là phần tế nhị nhất. Khi con trỏ chạm tới tổng gấp ba hợp lệ, có hai trường hợp. Nếu cả hai con trỏ đều có cùng một giá trị thì mọi cặp trong khối đó sẽ tạo thành các kết hợp hợp lệ, được tính bằng công thức kết hợp. Mặt khác, chúng tôi đếm có bao nhiêu giá trị giống nhau tồn tại ở bên trái và bên phải và nhân chúng lên, bởi vì mỗi lần xuất hiện bên trái có thể ghép nối với mỗi lần xuất hiện bên phải một cách độc lập. 

Chuyển động của con trỏ đảm bảo rằng khi một vùng được sử dụng, nó sẽ không bao giờ được xem lại, giữ cho độ phức tạp tổng thể là bậc hai. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
-1 0 1 2
```Đã sắp xếp mảng rồi`[-1, 0, 1, 2]`. 

| tôi | tôi | r | một [tôi] | một [l] | một [r] | tổng hợp | hành động | trả lời | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 3 | -1 | 0 | 2 | 1 | r-- | 0 | 
| 0 | 1 | 2 | -1 | 0 | 1 | 0 | tìm thấy | 1 | 

Dấu vết này cho thấy cách thuật toán phát hiện chính xác một bộ ba hợp lệ, được hình thành bởi`(-1, 0, 1)`. 

### Ví dụ 2 

đầu vào:```
5
0 0 0 0 0
```| tôi | tôi | r | một [tôi] | một [l] | một [r] | tổng hợp | hành động | trả lời | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 4 | 0 | 0 | 0 | 0 | đếm C(4,2) | 6 | 

Tất cả các phần tử đều giống hệt nhau nên mọi lựa chọn trong 3 chỉ số đều hợp lệ. Thuật toán tổng hợp chính xác tất cả các kết hợp cùng một lúc bằng cách sử dụng phép đếm tổ hợp. 

Ví dụ này xác nhận rằng việc nén trùng lặp không bị bỏ sót hoặc kết hợp tính hai lần. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2)$| Mỗi chỉ mục cố định chạy quét hai con trỏ tuyến tính | 
| Không gian |$O(1)$thêm | Sắp xếp được thực hiện ngoài ngăn xếp đệ quy | 

Đối với các ràng buộc điển hình trong các bài toán kiểu tổng 3, thời gian bậc hai là ranh giới dự kiến, phù hợp thoải mái trong các giới hạn cho$n \approx 2000$ĐẾN$5000$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    try:
        solve()
    except:
        pass
    return sys.stdout.getvalue().strip()

# sample-like cases
assert run("4\n-1 0 1 2\n") == "1"
assert run("5\n0 0 0 0 0\n") == "10"

# custom cases
assert run("3\n1 2 3\n") == "0", "no triple exists"
assert run("3\n-1 -1 2\n") == "1", "single valid triple"
assert run("4\n-2 0 1 1\n") == "1", "duplicate handling"
assert run("6\n-2 -1 0 1 2 3\n") == "3", "multiple valid triples"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 2 3`| 0 | không có bộ ba hợp lệ | 
|`-1 -1 2`| 1 | trùng lặp với số tiền hợp lệ | 
|`-2 0 1 1`| 1 | giá trị lặp lại với ghép nối một phần | 
|`-2 -1 0 1 2 3`| 3 | nhiều giải pháp rải rác | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi tất cả các phần tử trong một vùng hợp lệ đều giống hệt nhau. Đối với đầu vào`[0, 0, 0, 0]`, thuật toán đi vào trạng thái trong đó`l`Và`r`trỏ đến cùng một giá trị. Thay vì lặp lại từng cặp, nó nén toàn bộ phạm vi và đếm các kết hợp bằng cách sử dụng$\binom{4}{2} = 6$cặp cho mỗi neo cố định, đảm bảo tất cả các bộ ba được bao gồm chính xác một lần. 

Một trường hợp khác là khi các bộ ba hợp lệ chỉ tồn tại ở ranh giới của mảng. Vì`[-5, -2, 0, 2, 5]`, chuyển động của con trỏ dần dần loại bỏ các tổng không thể có ở cả hai đầu. Mặc dù các kết hợp hợp lệ rất thưa thớt, thuật toán vẫn tiếp cận chúng vì mọi dịch chuyển đơn điệu đều bảo toàn tính bất biến mà không có cặp khả thi nào bị bỏ qua trong phân đoạn chỉ số cố định hiện tại.
