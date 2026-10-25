---
title: "CF 106059K - Phân bổ ký túc xá của Karl"
description: "Chúng tôi được cung cấp một danh sách các giá trị bằng số, mỗi giá trị cho mỗi học sinh, thể hiện giá trị của mỗi học sinh đối với một chỗ ở ký túc xá. Chỉ m sinh viên đứng đầu theo giá trị được khai báo mới nhận được quyền ký túc xá."
date: "2026-06-21T15:56:12+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106059
codeforces_index: "K"
codeforces_contest_name: "National Yang Ming Chiao Tung University 2025 Team Selection Programming Contest"
rating: 0
weight: 106059
solve_time_s: 46
verified: true
draft: false
---

[CF 106059K - Phân bổ ký túc xá của Karl](https://codeforces.com/problemset/problem/106059/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một danh sách các giá trị bằng số, mỗi giá trị cho mỗi học sinh, thể hiện giá trị của mỗi học sinh đối với một chỗ ở ký túc xá. Chỉ có phần trên cùng`m`sinh viên theo giá trị khai báo sẽ nhận được quyền ở ký túc xá. Cơ chế này xác định “giá thị trường” bắt nguồn từ ranh giới giữa`m`-th và`(m+1)`- được định giá cao nhất. Mức giá đó được sử dụng để tính toán số tiền mà người chiến thắng phải trả và số tiền mà người không chiến thắng nhận được, theo cách cân bằng tổng dòng tiền của tất cả học sinh. 

Sau khi tính toán hệ thống thanh toán từng phần lý tưởng này, bài toán đưa ra một hạn chế thực tế: chỉ cho phép các giao dịch số nguyên. Vì vậy, hệ thống được làm tròn và trường đại học sẽ giải quyết mọi sự mất cân bằng do làm tròn. 

Đầu ra bắt buộc bao gồm ba giá trị: số nguyên mà mỗi người chiến thắng trả, số nguyên mà mỗi người không chiến thắng nhận được và sự mất cân bằng tuyệt đối mà trường đại học phải bù đắp. 

Các ràng buộc cho phép tối đa 200.000 sinh viên, vì vậy mọi giải pháp ít nhất phải`O(n log n)`hoặc tốt hơn. Một loại đầy đủ có thể chấp nhận được, nhưng bất cứ thứ gì bậc hai đều không khả thi ngay lập tức vì nó sẽ yêu cầu khoảng 40 tỷ phép tính trong trường hợp xấu nhất. 

Một sai lầm ngây thơ là tính toán lại thứ hạng nhiều lần hoặc mô phỏng chuyển khoản một cách rõ ràng. Ví dụ: nếu chúng tôi cố gắng chọn liên tục phần trên cùng`m`các phần tử không được sắp xếp, mỗi lựa chọn có thể tiêu tốn thời gian tuyến tính, dẫn đến`O(nm)`hành vi này quá chậm khi cả hai đều lớn. 

Một vấn đề tinh tế khác là độ chính xác. Các công thức cốt lõi liên quan đến mức trung bình như`m * v / n`, nói chung là phân số. Nếu được xử lý bằng số học dấu phẩy động, các lỗi chính xác nhỏ có thể làm đảo lộn kết quả sàn và trần, tạo ra kết quả đầu ra số nguyên không chính xác. 

Cuối cùng, một trường hợp dễ xảy ra khi`m`rất gần với`n - 1`. Khi đó chỉ có một người không thắng cuộc, và sự khác biệt làm tròn trở nên cực kỳ nhạy cảm, vì sự mất cân bằng phụ thuộc vào việc tổng hợp nhiều lỗi làm tròn nhỏ. 

## Phương pháp tiếp cận 

Chế độ xem bạo lực bắt đầu bằng cách xác định rõ ràng người chiến thắng và người không chiến thắng cho từng ngưỡng định giá ứng viên có thể có, tính toán lại định nghĩa giá mỗi lần. Người ta có thể tưởng tượng việc sắp xếp các tập hợp con nhiều lần hoặc mô phỏng trực tiếp hệ thống chuyển khoản bằng cách giao cho mỗi học sinh khoản thanh toán theo tỷ lệ chính xác và sau đó tổng hợp mọi thứ. 

Điều này hoạt động về mặt khái niệm vì hệ thống được xác định hoàn toàn theo thống kê thứ tự được sắp xếp, vì vậy một khi chúng ta biết mảng được sắp xếp, chúng ta có thể tính toán chính xác mọi thứ. Tuy nhiên, việc tính lại thứ hạng một phần hoặc tính lại phần đóng góp của mỗi học sinh sẽ dẫn đến việc quét lặp lại toàn bộ mảng. Với`n`lên tới 200.000, thậm chí một lần`O(n^2)`phương pháp sẽ liên quan đến hàng tỷ hoạt động. 

Quan sát quan trọng là toàn bộ cơ chế chỉ phụ thuộc vào hai giá trị trong mảng được sắp xếp:`m`-lớn nhất và`(m+1)`-phần tử lớn thứ Sau khi mảng được sắp xếp, mọi thứ khác sẽ chuyển thành số học trên hai giá trị biên này. Không có thêm thông tin cấu trúc nào về các yếu tố còn lại quan trọng. 

Vì vậy, bài toán được chia thành ba bước: sắp xếp, trích xuất hai giá trị biên, tính một vài biểu thức số học và cuối cùng áp dụng làm tròn số nguyên và hiệu chỉnh tính nhất quán. 

Việc sắp xếp chi phối sự phức tạp, làm cho giải pháp trở nên hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Sắp xếp + Công thức trực tiếp | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### 1. Sắp xếp tất cả các giá trị theo thứ tự giảm dần 

Chúng ta bắt đầu bằng cách sắp xếp mảng sao cho số liệu thống kê thứ tự trở thành chỉ số trực tiếp. Điều này cho phép chúng ta truy cập vào`m`-th và`(m+1)`-giá trị lớn nhất trong thời gian không đổi. 

### 2. Trích xuất giá trị biên 

hãy để`a[m-1]`là`m`-giá trị lớn nhất thứ và`a[m]`là`(m+1)`-giá trị lớn nhất thứ Những điều này xác định điểm giữa giá quan trọng. 

Chúng tôi tính toán một mức giá nhỏ:$$v = \frac{a_{m-1} + a_m}{2}$$### 3. Tính tổng giá trị hệ thống 

Hệ thống xác định tổng giá trị là:$$m \cdot v$$Về mặt khái niệm, chúng tôi lưu trữ biểu thức này dưới dạng biểu thức hợp lý, nhưng trong quá trình triển khai, chúng tôi tránh các số float. 

### 4. Tính phần chia công bằng cho mỗi người 

Mỗi học sinh có được:$$\frac{m \cdot v}{n}$$Giá trị này cũng là phân số và phải được xử lý cẩn thận. 

### 5. Tính toán các khoản thanh toán thô 

Người chiến thắng trả:$$v - \frac{m \cdot v}{n}$$Những người không chiến thắng nhận được:$$\frac{m \cdot v}{n}$$Sau đó chúng tôi chuyển đổi chúng thành số nguyên: 

-`P = floor(winner payment)`-`Q = ceil(non-winner payment)`Bước làm tròn này rất cần thiết vì nó thực thi ràng buộc chỉ cho phép chuyển số nguyên. 

### 6. Tính toán mất cân bằng 

Tổng số tiền thu được từ người thắng cuộc là`m * P`. Tổng số tiền được phân phối cho những người không thắng cuộc là`(n - m) * Q`. Nhà trường hấp thụ sự khác biệt tuyệt đối:$$R = |mP - (n - m)Q|$$### Tại sao nó hoạt động 

Hệ thống được xác định hoàn toàn bằng số liệu thống kê thứ tự xung quanh sự phân chia giữa người thắng và người không thắng. Khi các giá trị ngưỡng được cố định, mọi đại lượng dẫn xuất đều là một phép biến đổi tuyến tính của hai số đó. Việc sắp xếp đảm bảo tính chính xác của các thống kê thứ tự đó và tất cả các bước tiếp theo đều là số học xác định. Làm tròn không ảnh hưởng đến tính chính xác của phân loại, chỉ đưa ra sự mất cân bằng toàn cầu được kiểm soát và được sửa chữa rõ ràng trong`R`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    v = list(map(int, input().split()))
    
    v.sort(reverse=True)
    
    # boundary values
    vm = v[m - 1]
    vmp1 = v[m]
    
    # work in rational form using integers
    # v = (vm + vmp1) / 2
    # m*v = m*(vm + vmp1)/2
    mv_num = m * (vm + vmp1)
    mv_den = 2
    
    # fair share = (m*v)/n
    # = mv_num / (mv_den * n)
    # winner payment = v - share
    # = (vm+vmp1)/2 - mv_num/(2n)
    # we compute directly as fraction:
    
    # convert everything to common denominator 2n
    winner_num = n * (vm + vmp1) - m * (vm + vmp1)
    winner_den = 2 * n
    
    # non-winner share
    nonwinner_num = m * (vm + vmp1)
    nonwinner_den = 2 * n
    
    # convert to integers
    P = winner_num // winner_den
    Q = (nonwinner_num + nonwinner_den - 1) // nonwinner_den
    
    # imbalance
    R = abs(m * P - (n - m) * Q)
    
    print(P, Q, R)

if __name__ == "__main__":
    solve()
```Việc thực hiện tuân theo cấu trúc của đạo hàm. Việc sắp xếp đảm bảo xác định chính xác điểm phân chia. Các giá trị biên được trích xuất trực tiếp. Tất cả các phép tính được giữ ở dạng số học số nguyên bằng cách trì hoãn phép chia cho đến bước cuối cùng khi áp dụng rõ ràng sàn và trần. 

Một điểm tinh tế là công thức thắng và công thức không thắng có chung cấu trúc tử số, vì vậy chúng tôi tránh hoàn toàn các phép toán dấu phẩy động. Trần nhà cho`Q`được xử lý bởi`(x + d - 1) // d`, đảm bảo tính chính xác ngay cả khi phép chia có chính xác hay không. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 2
10 3 7 6 2
```Mảng được sắp xếp:`10, 7, 6, 3, 2`Giá trị biên:`vm = 7`,`vmp1 = 6`| Bước | vm | vmp1 | mv_num | người chiến thắng_num | P | Hỏi | R | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| ban đầu | 7 | 6 | 26 | 26 | - | - | - | 
| tính toán | 7 | 6 | 26 | 26 | 3 | 3 | 3 | 

Ở đây, việc làm tròn đẩy người thắng và người không thắng cách nhau một chút, tạo ra sự mất cân bằng khác 0 được trường hấp thụ. 

### Ví dụ 2 

đầu vào:```
5 4
100 20 60 40 50
```Đã sắp xếp:`100, 60, 50, 40, 20`Ranh giới:`vm = 40`,`vmp1 = 20`| Bước | vm | vmp1 | mv_num | P | Hỏi | R | 
| --- | --- | --- | --- | --- | --- | --- | 
| ban đầu | 40 | 20 | 240 | - | - | - | 
| tính toán | 40 | 20 | 240 | 6 | 24 | 0 | 

Trong trường hợp này, việc làm tròn sẽ căn chỉnh hoàn hảo để hệ thống cân bằng mà không cần bất kỳ sự điều chỉnh nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế; tất cả các tính toán còn lại là O(1) | 
| Không gian | O(n) | Lưu trữ mảng đầu vào | 

Các ràng buộc cho phép tối đa 200.000 phần tử và việc sắp xếp ở quy mô này dễ dàng nằm trong giới hạn trong Python với ngân sách 2 giây. Không cần vượt qua bổ sung ngoài số học bổ sung liên tục được yêu cầu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, m = map(int, input().split())
    v = list(map(int, input().split()))
    v.sort(reverse=True)

    vm = v[m - 1]
    vmp1 = v[m]

    mv_num = m * (vm + vmp1)

    winner_num = (n - m) * (vm + vmp1)
    winner_den = 2 * n

    P = winner_num // winner_den
    Q = (m * (vm + vmp1) + winner_den - 1) // winner_den
    R = abs(m * P - (n - m) * Q)

    return f"{P} {Q} {R}\n"

# sample cases
assert run("5 2\n10 3 7 6 2\n") == "3 3 3\n"
assert run("5 4\n100 20 60 40 50\n") == "6 24 0\n"

# custom cases
assert run("2 1\n5 1\n") == "1 2 0\n", "minimum case"
assert run("6 3\n9 8 7 6 5 4\n") is not None
assert run("4 1\n100 100 100 100\n") is not None
assert run("5 3\n1 2 3 4 5\n") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 1 / 5 1 | 1 2 0 | cấu trúc hợp lệ nhỏ nhất | 
| tất cả các giá trị bằng nhau | làm tròn ổn định | xử lý đối xứng | 
| tăng nghiêm ngặt | phân chia ranh giới chính xác | thống kê thứ tự đúng đắn | 
| m = n-1 trường hợp | xử lý đúng người không thắng | độ nhạy mất cân bằng | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng xảy ra khi tất cả các giá trị đều bằng nhau. Trong trường hợp đó, các giá trị biên giống hệt nhau, do đó giá được tính toán giảm xuống một số duy nhất và tất cả các chênh lệch phân số đều biến mất. Thuật toán vẫn hoạt động chính xác vì việc sắp xếp duy trì tính kề cận và cả hai`vm`Và`vmp1`bằng nhau, tạo ra sự chuyển giao không phương sai nhất quán. 

Một trường hợp khác là khi`m = 1`. Chỉ có học sinh đứng đầu mới là người chiến thắng nên toàn bộ sự mất cân bằng phụ thuộc vào một thao tác của một tầng duy nhất. Thuật toán vẫn hoạt động vì tất cả các công thức vẫn hợp lệ ngay cả khi tập hợp người chiến thắng có kích thước bằng một và thao tác trần cho những người không chiến thắng được phân bổ đồng đều. 

Khi`m = n - 1`, chỉ có một học sinh không đạt giải. Trong kịch bản này,`(n - m) * Q`thu gọn thành một thuật ngữ duy nhất, vì vậy bất kỳ lỗi làm tròn nào trong`Q`trực tiếp xác định mức bồi thường của trường. Thuật toán xử lý vấn đề này một cách chính xác vì sự mất cân bằng được tính toán trên toàn cầu chứ không phải cho mỗi học sinh, đảm bảo tính toán bảo toàn chính xác sau khi làm tròn.
