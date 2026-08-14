---
title: "CF 102319G - Jonathan và Jason tại Jowling Jalley I"
description: "Chúng ta có một sự sắp xếp các chốt theo hình tam giác với n chốt ở hàng dưới cùng, n - 1 ở hàng phía trên nó, v.v., cho tổng số [ 1+2+dots+n=frac{n(n+1)}2 ] ghim. Sau khi bóng lăn, chốt duy nhất mà quả bóng có thể đánh đổ là chốt trên cùng."
date: "2026-08-13T04:57:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102319
codeforces_index: "G"
codeforces_contest_name: "UBC Summer Contest 2018"
rating: 0
weight: 102319
solve_time_s: 477
verified: true
draft: false
---

[CF 102319G – Jonathan và Jason tại Jowling Jalley I](https://codeforces.com/problemset/problem/102319/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 7 phút 57 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có sự sắp xếp các chốt theo hình tam giác với`n`ghim ở hàng dưới cùng,`n - 1`ở hàng phía trên nó, v.v., tổng cộng là\[
1+2+\dots+n=\frac{n(n+1)}2
\]ghim. 

Sau khi bóng lăn, chốt duy nhất mà quả bóng có thể đánh đổ là chốt trên cùng. Mọi chiếc ghim bị đổ khác chắc chắn là do những chiếc ghim ngay trước nó gây ra. Một chốt chỉ có thể rơi khi hai chốt đỡ nó ở hàng trước đó đã rơi xuống, với cách giải thích ranh giới tự nhiên cho các chốt ở hai bên. 

Nhiệm vụ không phải là tìm ra một sự sắp xếp cuối cùng cụ thể nào. Chúng ta cần đếm mọi sự sắp xếp riêng biệt có thể xảy ra sau một lần tung. 

Đầu vào chứa độ dài cạnh`n`, Ở đâu`4 <= n <= 20`. Hình tam giác lớn nhất chỉ chứa 210 chân nên việc lưu trữ một cấu hình rất dễ dàng. Khó khăn là 210 lựa chọn nhị phân độc lập sẽ cho\(2^{210}\)các tập hợp con có thể, đại khái\(1.6 \times 10^{63}\). Việc liệt kê trực tiếp vượt xa mọi thứ có thể chạy trong một giây. Giá trị nhỏ của`n`chỉ hữu ích sau khi chúng ta tìm thấy cấu trúc tổ hợp của các cấu hình hợp lệ. 

Có hai trường hợp cạnh rất dễ xử lý sai. Vì`n = 4`, câu trả lời là`42`, không`2^10 = 1024`, bởi vì hầu hết tất cả các tập con đều vi phạm quy luật nhân quả. Các cấu hình hợp lệ chính xác là các đối tượng Catalan được liên kết với cấu trúc phụ thuộc hình tam giác này. Ở đầu bên kia,`n = 20`có 210 chân, nhưng câu trả lời vẫn vừa vặn với số nguyên 64 bit:\[
C_{21}=24466267020.
\]Việc triển khai bất cẩn sử dụng số nguyên 32 bit có chiều rộng cố định sẽ tràn vào đầu vào này mặc dù bản thân đầu vào đó rất nhỏ. Số nguyên có độ chính xác tùy ý của Python tránh được vấn đề đó. 

## Phương pháp tiếp cận 

Một giải pháp trực tiếp sẽ liệt kê mọi tập hợp con của các vị trí 210 chân. Đối với mỗi tập hợp con, chúng tôi sẽ kiểm tra từng mã pin bị loại bỏ và xác minh rằng các mã pin trước đó được yêu cầu cũng bị loại bỏ. Điều này đúng vì mọi kết quả có thể có về mặt vật lý chỉ đơn giản là một tập hợp con hợp lệ và việc kiểm tra điều kiện trước đó khớp chính xác với các quy tắc. 

Vấn đề là số lượng tập hợp con. Với các chân \(N=\frac{n(n+1)}2\), lực lượng vũ phu sẽ mất khoảng\(2^N\)kiểm tra cấu hình và thậm chí kiểm tra liên tục theo thời gian cho mỗi cấu hình sẽ là vô vọng. Ở mức tối đa`n = 20`, đây là\(2^{210}\), khoảng\(1.64\times10^{63}\)cấu hình. Việc thêm xác thực \(O(N)\) cho mỗi cấu hình khiến cách tiếp cận trở nên tồi tệ hơn. 

Quan sát quan trọng là các tập hợp loại hợp lệ có cấu trúc mạnh hơn nhiều so với các tập hợp con tùy ý. Nếu một chốt rơi xuống, mọi chốt nằm phía trên nó dọc theo chuỗi phụ thuộc cũng phải rơi xuống. Nói cách khác, các chốt rơi tạo thành một trật tự lý tưởng của bộ phụ thuộc tam giác. Một lý tưởng như vậy được mô tả đầy đủ bởi ranh giới của nó. 

Nếu chúng ta vẽ ranh giới ngăn cách các chốt rơi khỏi các chốt thẳng đứng, ranh giới đó có thể được mã hóa thành một đường đi đơn điệu. Điều kiện phụ thuộc ngăn không cho đường đi đi qua chính nó hoặc rời khỏi vùng tam giác, và sau phần đệm thông thường ở hai đầu, các ranh giới này chính xác là các đường đi Dyck có bán dài`n + 1`. 

Số đường đi Dyck có bán độ dài`m`là`m`-số Catalan thứ,\[
C_m=\frac{1}{m+1}\binom{2m}{m}.
\]Đây`m = n + 1`, vậy đáp án cần tìm là\[
\boxed{C_{n+1}
=\frac{1}{n+2}\binom{2n+2}{n+1}}.
\]Brute Force hoạt động vì nó xem xét mọi trạng thái có thể và từ chối những trạng thái không hợp lệ, nhưng không thành công vì không gian trạng thái theo cấp số nhân theo số lượng chân. Việc quan sát ranh giới thu gọn tất cả các trạng thái đó thành một họ Catalan tiêu chuẩn, giảm việc tính toán thành một vòng lặp số học ngắn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
|---|---|---|---| 
| Lực lượng vũ phu | \(O(n^2 2^{n(n+1)/2})\) | \(O(n^2)\) | Quá chậm | 
| Công thức Catalan | \(O(n)\) | \(O(1)\) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Hãy để`m = n + 1`. Các cấu hình chân hợp lệ tương ứng một-một với các đường dẫn Dyck có`m`bước mở đầu và`m`các bước chốt. Cái thêm vào trong`n + 1`xuất phát từ hai cạnh của ranh giới tam giác cùng với sự phụ thuộc trên cùng, do đó ranh giới chốt được biểu diễn bằng đường đi Dyck có chiều dài bán phần lớn hơn chiều dài cạnh. 

2. Tính số Catalan\[
   C_m=\frac{1}{m+1}\binom{2m}{m}.
   \]Chúng ta có thể tính hệ số nhị thức bằng các giai thừa, nhưng phép truy toán nhân đơn giản hơn và tránh được việc xây dựng các giai thừa trung gian lớn. 

3. Bắt đầu với`C_0 = 1`và sử dụng\[
   C_k=C_{k-1}\frac{2(2k-1)}{k+1}.
   \]Mọi phép chia đều chính xác, vì đây là phép hồi quy số nguyên tiêu chuẩn cho số Catalan. 

4. Tiếp tục cho đến khi`k = n + 1`, sau đó in`C_{n+1}`. 

### Tại sao nó hoạt động 

Mọi cấu hình hợp lệ đều có đặc tính là một chốt rơi sẽ buộc tất cả các cấu hình trước đó của nó bị rơi. Do đó, vùng rơi không thể có hình dạng tùy ý. Biên của nó di chuyển đơn điệu theo cách sắp xếp tam giác, và điều kiện phụ thuộc chính xác là điều kiện đường biên tương ứng không bao giờ đi qua đường chéo cấm. Do đó, mỗi cấu hình chân hợp lệ sẽ cung cấp một đường dẫn Dyck và mỗi đường dẫn Dyck như vậy sẽ tái tạo lại một cấu hình hợp lệ. Vì đường đi Dyck có bán độ dài`n + 1`được tính bằng\(C_{n+1}\), thuật toán trả về chính xác số vị trí cuối cùng có thể có về mặt vật lý. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    m = n + 1

    # Catalan number C_m.
    cat = 1
    for k in range(1, m + 1):
        cat = cat * 2 * (2 * k - 1) // (k + 1)

    print(cat)

if __name__ == "__main__":
    solve()
```Đầu vào bao gồm một số nguyên duy nhất, vì vậy`solve`đọc nó trực tiếp. Cài đặt`m = n + 1`chuyển đổi hình dạng của tam giác chốt thành chỉ số Catalan tương ứng. 

Biến`cat`lưu trữ số Catalan hiện tại. Ban đầu nó tượng trưng\(C_0=1\). Tại lần lặp`k`, sự tái phát biến nó thành\(C_k\). Phép nhân được thực hiện trước phép chia và số nguyên của Python có độ chính xác tùy ý, do đó không có rủi ro tràn. 

Vòng lặp kết thúc tại`n + 1`, khớp chính xác với chỉ mục được yêu cầu bởi sự tương ứng tổ hợp. Không có mảng và không có đệ quy, do đó việc triển khai sử dụng không gian phụ trợ không đổi. 

## Ví dụ đã hoạt động 

Tuyên bố cung cấp một mẫu,`n = 4`. Vì không có mẫu chính thức thứ hai trong tuyên bố được cung cấp nên dấu vết thứ hai sử dụng`n = 5`. 

Vì`n = 4`, chúng tôi cần\(C_5\). 

|`k`| Giá trị Catalan hiện tại | 
|---:|---:| 
| 0 | 1 | 
| 1 | 1 | 
| 2 | 2 | 
| 3 | 5 | 
| 4 | 14 | 
| 5 | 42 | 

Giá trị cuối cùng là`42`, phù hợp với mẫu chính thức. Dấu vết cho thấy mười chân không dẫn đến\(2^{10}\)các trạng thái tùy ý. Quy tắc phụ thuộc làm giảm chúng xuống số Catalan thứ năm. 

Vì`n = 5`, chúng tôi cần\(C_6\). 

|`k`| Giá trị Catalan hiện tại | 
|---:|---:| 
| 0 | 1 | 
| 1 | 1 | 
| 2 | 2 | 
| 3 | 5 | 
| 4 | 14 | 
| 5 | 42 | 
| 6 | 132 | 

Đầu ra là`132`. Đây là phép lặp tương tự được sử dụng cho mọi đầu vào hợp lệ, do đó việc tăng tam giác lên một hàng chỉ yêu cầu một lần lặp số học bổ sung. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
|---|---|---| 
| Thời gian | \(O(n)\) | Phép tái diễn Catalan được đánh giá cho`n + 1`các giá trị. | 
| Không gian | \(O(1)\) | Chỉ giá trị Catalan hiện tại và một vài số nguyên được lưu trữ. | 

Với`n <= 20`, thuật toán chỉ thực hiện 21 lần lặp. Kết quả lớn nhất là\(C_{21}=24466267020\), mà Python đại diện chính xác. Giải pháp này thấp hơn nhiều so với giới hạn thời gian một giây và sử dụng bộ nhớ không đáng kể. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve_value(n: int) -> str:
    m = n + 1
    cat = 1

    for k in range(1, m + 1):
        cat = cat * 2 * (2 * k - 1) // (k + 1)

    return str(cat)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    n = int(sys.stdin.readline())
    result = solve_value(n)

    sys.stdin = old_stdin
    return result + "\n"

# Provided sample
assert run("4\n") == "42\n", "sample 1"

# Minimum allowed input
assert run("4\n") == "42\n", "minimum n"

# Small consecutive value
assert run("5\n") == "132\n", "Catalan C6"

# Another boundary-style case
assert run("6\n") == "429\n", "Catalan C7"

# Maximum allowed input
assert run("20\n") == "24466267020\n", "maximum n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
|---|---:|---| 
|`4`|`42`| Mẫu chính thức và mức tối thiểu cho phép`n`| 
|`5`|`132`| Chuyển đúng từ`C_n`ĐẾN`C_{n+1}`| 
|`6`|`429`| Giá trị Catalan liên tiếp và phép truy hồi | 
|`20`|`24466267020`| Ràng buộc tối đa và số học số nguyên lớn | 

## Vỏ cạnh 

cho`n = 4`, hình tam giác chỉ có mười chiếc ghim, có thể khiến cho lực lượng vũ phu trông thật hấp dẫn. Thuật toán không liệt kê những\(1024\)tập hợp con. Nó tính toán trực tiếp\(C_5\), sản xuất`42`. Sự khác biệt quan trọng vì phần lớn các tập con tùy ý vi phạm điều kiện trước đó. 

Vì`n = 20`, có 210 chân, vì vậy việc liệt kê các cấu hình sẽ yêu cầu phải xem xét\(2^{210}\)tập hợp con. Thay vào đó, thuật toán tính toán\(C_{21}\)qua 21 bước lặp lại. Các giá trị vẫn chính xác, kết thúc tại`24466267020`. 

Việc lập chỉ mục là lỗi số học dễ mắc phải nhất. sử dụng\(C_n\)sẽ cho`14`vì`n = 4`, trong khi kết quả đúng là`42`. Ranh giới tam giác tương ứng với các đường Dyck có bán độ dài`n + 1`, do đó việc thực hiện có chủ ý thiết lập`m = n + 1`trước khi đánh giá sự tái phát. 

Sự truy hồi cũng cần mẫu số của nó là`k + 1`. Ví dụ, tại`k = 5`,\[
C_5=C_4\frac{2(2\cdot5-1)}{5+1}
=14\cdot\frac{18}{6}
=42.
\]sử dụng`k`thay vào đó sẽ âm thầm tạo ra các giá trị không chính xác mặc dù tất cả các phép toán vẫn là số nguyên. Việc triển khai tuân theo chính xác phép truy toán Catalan tiêu chuẩn, do đó điều kiện biên này được xử lý mà không có trường hợp đặc biệt nào. 

:::
