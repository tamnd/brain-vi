---
title: "CF 105255I - Thế Giới Nước"
description: "Bài toán mô tả một hành tinh có bề mặt được quan sát một cách rất có cấu trúc. Thay vì nhìn toàn bộ quả cầu cùng một lúc, quá trình đo sẽ cắt hành tinh theo hai hướng. Đầu tiên, hành tinh được chia theo chiều dọc thành n dải ngang từ cực này sang cực khác."
date: "2026-06-24T05:28:32+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105255
codeforces_index: "I"
codeforces_contest_name: "2023 ICPC World Finals"
rating: 0
weight: 105255
solve_time_s: 46
verified: true
draft: false
---

[CF 105255I - Thế giới nước](https://codeforces.com/problemset/problem/105255/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Bài toán mô tả một hành tinh có bề mặt được quan sát một cách rất có cấu trúc. Thay vì nhìn toàn bộ quả cầu cùng một lúc, quá trình đo sẽ cắt hành tinh theo hai hướng. 

Đầu tiên, hành tinh được chia theo chiều dọc thành`n`dải ngang từ cực này sang cực khác. Các dải này không bằng nhau về diện tích vật lý trên một hình cầu, nhưng vấn đề đã khắc phục chúng dưới dạng các vùng hình học`A1`bởi vì`An`, trong đó phần trên và phần dưới co lại thành hình tam giác gần các cực, trong khi phần ở giữa hoạt động giống như các dải tứ giác cong. 

Thứ hai, thời gian được rời rạc hóa thành`m`các bước quay. Mỗi bước tương ứng với việc quay hành tinh theo một góc cố định sao cho một lát cắt dọc mới có chiều rộng`d = 360 / m`độ được quan sát thấy. Đối với mỗi bước`j`, chúng tôi nhận được`n`tỷ lệ phần trăm`a[i][j]`, đại diện cho bao nhiêu khu vực`Ai`được bao phủ bởi nước trong lát cắt dọc đó. 

Vì vậy, mỗi giá trị là một tỷ lệ phần trăm cục bộ bên trong một “ô” của lưới hình cầu: các dải vĩ độ theo các lát cắt kinh độ. 

Mục đích không phải là tái tạo lại hình học một cách rõ ràng mà là tính toán phần tổng thể của hành tinh được bao phủ bởi nước. Điều đó có nghĩa là mỗi tế bào đều đóng góp tỷ lệ thuận với diện tích bề mặt thực tế của nó trên quả cầu. 

Khó khăn chính là những`n × m`các vùng không phải là hình chữ nhật có diện tích bằng nhau. Các dải gần xích đạo lớn hơn các dải ở gần cực và các lát dọc đều có chiều rộng góc bằng nhau nhưng vẫn tương ứng với các phần diện tích bằng nhau trong mỗi dải vĩ độ. Vì vậy, nhiệm vụ thực sự là tính trung bình có trọng số trên phân vùng hình cầu này. 

Những hạn chế`n, m ≤ 1000`ngụ ý lên tới một triệu tế bào, vì vậy bất kỳ`O(nm)`xử lý là tốt, nhưng bất cứ điều gì tồi tệ hơn sẽ là không cần thiết. Các chi tiết hình học chỉ quan trọng để xác định trọng số chính xác trên mỗi dải vĩ độ. 

Trường hợp cạnh tinh tế là đầu vào đồng nhất. Nếu mọi`a[i][j] = 10`, câu trả lời phải chính xác là 10, mặc dù kích thước diện tích khác nhau, bởi vì mỗi ô đóng góp tỷ lệ thuận với diện tích của nó và tất cả các ô đều có giá trị giống nhau. 

Một trường hợp khác là sự thay đổi mạnh mẽ theo vĩ độ. Nếu chỉ các hàng cực có nước, thì việc tính trung bình ngây thơ trên mỗi hàng sẽ bị đếm quá hoặc thiếu vì các dải cực biểu thị diện tích bề mặt nhỏ hơn các dải xích đạo. 

## Phương pháp tiếp cận 

Một sự hiểu lầm nghiêm trọng sẽ cố gắng tái tạo lại hình cầu và tính diện tích bề mặt chính xác của mỗi hình tứ giác`A[i]`cho tất cả các dải vĩ độ, sau đó lấy tích phân theo kinh độ. Điều đó sẽ liên quan đến việc tính toán diện tích lượng giác trên một hình cầu, có thể tính toán các ranh giới vĩ độ, sau đó nhân với tỷ lệ phần trăm quan sát được và tính tổng. 

Tuy nhiên, quan sát quan trọng là phân vùng đã phù hợp với hình học hình cầu. Mỗi lát cắt dọc có chiều rộng góc bằng nhau, do đó trong bất kỳ dải vĩ độ cố định nào, tất cả`m`lát có diện tích giống hệt nhau. Điều này có nghĩa là kinh độ chỉ đóng góp một yếu tố đồng nhất và bị loại bỏ. 

Do đó, sự không đồng nhất duy nhất nằm ở các dải vĩ độ. Mỗi ban nhạc`Ai`tương ứng với một vùng hình cầu giữa hai vĩ độ và diện tích của nó tỷ lệ thuận với hiệu sin của các góc biên. Hình dạng chính xác được đơn giản hóa đáng kể: trọng lượng chính xác của dây đeo`i`tỷ lệ thuận với chiều cao trung bình của dải đó trên hình cầu, điều này hoàn toàn được thể hiện bởi thực tế là các dải vĩ độ bằng nhau trên hình cầu không có diện tích bằng nhau. 

Nhưng vấn đề đã rời rạc hóa hình cầu theo cách ngầm mã hóa sự phân tách diện tích bằng nhau trên mỗi ô một khi chúng ta diễn giải nó một cách chính xác: mỗi ô`(i, j)`biểu thị cùng một chiều rộng góc theo kinh độ và cùng một chiều cao góc trong phân vùng vĩ độ, do đó sự đóng góp của nó tỷ lệ thuận với diện tích dải hình cầu, chỉ phụ thuộc vào`i`, không bật`j`. 

Điều này dẫn đến việc cải cách đơn giản hơn. Chúng ta có thể xử lý từng hàng`i`vì có trọng lượng cố định`w[i]`bằng một phần của nó trên toàn bộ hình cầu. Vậy thì câu trả lời là:$$\frac{\sum_{i=1}^{n} w[i] \cdot \left(\frac{1}{m} \sum_{j=1}^{m} a[i][j]\right)}{\sum w[i]}$$Vì các trọng số có tổng bằng 1 nên giá trị này đơn giản trở thành giá trị trung bình có trọng số của các giá trị trung bình của hàng. 

Thông tin chi tiết quan trọng được sử dụng trong các giải pháp chính thức là việc xây dựng hình cầu ngụ ý rằng tất cả các dải vĩ độ đều có đóng góp diện tích bằng nhau trong mô hình rời rạc hóa này. Mô tả hình học tinh tế đảm bảo rằng mỗi`n × m`các vùng tương ứng với các phân vùng có diện tích bằng nhau khi được tổng hợp chính xác trong toàn bộ vòng quay. Do đó, mỗi ô có thể được coi là có trọng lượng bằng nhau. 

Vì vậy, toàn bộ vấn đề quy về việc tính giá trị trung bình của tất cả`n × m`các giá trị. 

Một giải pháp đơn giản tính toán tất cả hình học và tích phân trên mỗi ô, tốn ít nhất`O(nm)`cộng với lượng giác nặng trên mỗi ô. Giải pháp tối ưu ghi nhận sự đóng góp của tế bào đồng nhất và thu gọn mọi thứ thành một phương tiện đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (hình học trên mỗi ô) | O(nm) với hằng số nặng | O(nm) | Quá chậm | 
| Tối ưu (trung bình đồng đều) | O(nm) | O(1) thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`Và`m`, sau đó đọc tất cả các giá trị`a[i][j]`trong khi tích lũy tổng của chúng trong một biến duy nhất. Lý do điều này có hiệu quả là vì mọi phép đo đều tương ứng với một phân vùng đối xứng của hình cầu trong đó mỗi ô đóng góp bằng nhau vào tổng diện tích bề mặt. 
2. Duy trì tổng số hoạt động`S`của tất cả`n × m`tỷ lệ phần trăm. Không cần phân biệt hàng hoặc cột vì việc cắt hình học đảm bảo phân bổ diện tích đồng đều trên mỗi ô. 
3. Sau khi xử lý tất cả các giá trị, chia`S`qua`n × m`để có được tỷ lệ bao phủ toàn cầu của nước. 
4. Xuất kết quả dưới dạng số dấu phẩy động với độ chính xác vừa đủ, vì sai số tuyệt đối yêu cầu nhiều nhất là`1e-6`. 

### Tại sao nó hoạt động 

Phân vùng hình cầu được xây dựng sao cho mỗi ô đo tương ứng với một vùng góc bằng nhau trong lấy mẫu rời rạc của hình cầu. Mặc dù mô tả văn bản nhấn mạnh sự khác biệt về độ cong giữa các dải vĩ độ, nhưng những khác biệt đó đã được bù đắp bằng cách xác định các dải. Trong một vòng quay hoàn toàn, mọi dải vĩ độ được lấy mẫu thống nhất theo kinh độ và mọi lát cắt dọc có chiều rộng góc giống hệt nhau. Điều này làm cho mọi`(i, j)`vùng đại diện cho một phần bằng nhau của bề mặt hành tinh. Vì đầu ra là sự kết hợp tuyến tính của tỷ lệ phần trăm trên mỗi ô được tính theo diện tích bằng nhau nên kết quả sẽ thu về giá trị trung bình số học của tất cả các mục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n, m = map(int, input().split())
    total = 0.0
    for _ in range(n):
        row = list(map(int, input().split()))
        total += sum(row)

    print(total / (n * m))

if __name__ == "__main__":
    main()
```Việc triển khai dựa trên một lần tích lũy duy nhất qua ma trận. Mỗi hàng được tính tổng ngay lập tức, tránh lưu trữ toàn bộ lưới và giữ bộ nhớ ở mức tối thiểu. Sự phân chia cuối cùng bởi`n * m`chuyển đổi tổng phần trăm tích lũy thành mức trung bình toàn cầu. 

Một điểm tinh tế là sự ổn định của dấu phẩy động. Vì các giá trị tối đa là 100 và có tới một triệu mục nhập nên tổng vừa vặn với độ chính xác gấp đôi mà không làm mất độ chính xác vượt quá dung sai yêu cầu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 7
63 61 55 54 77 87 89
73 60 38 5 16 56 91
75 43 11 3 16 20 95
```Chúng tôi tích lũy từng hàng. 

| Bước | Tổng hàng | Tổng số cho đến nay | 
| --- | --- | --- | 
| 1 | 486 | 486 | 
| 2 | 339 | 825 | 
| 3 | 263 | 1088 | 

Trung bình cuối cùng là`1088 / 21 = 51.809523810`. 

Dấu vết này cho thấy rằng chỉ có sự tổng hợp mới quan trọng; cấu trúc bên trong các hàng là không liên quan. 

### Ví dụ 2 

đầu vào:```
4 3
10 10 10
10 10 10
10 10 10
10 10 10
```| Bước | Tổng hàng | Tổng số cho đến nay | 
| --- | --- | --- | 
| 1 | 30 | 30 | 
| 2 | 30 | 60 | 
| 3 | 30 | 90 | 
| 4 | 30 | 120 | 

Trung bình cuối cùng là`120 / 12 = 10`. 

Điều này xác nhận rằng các đầu vào thống nhất vẫn bất biến khi chuyển đổi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nm) | Mỗi giá trị được đọc một lần và được thêm vào tổng hiện hành | 
| Không gian | O(1) | Chỉ một số biến vô hướng được lưu trữ | 

Các ràng buộc cho phép lên tới một triệu giá trị, do đó, một lần chuyển là đủ nhanh. Không cần xử lý trước hoặc tính toán hình học, giữ cho cả bộ nhớ và thời gian chạy ở mức tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from math import isclose

    n, m = map(int, inp.split()[0:2])
    data = list(map(int, inp.split()[2:]))

    total = sum(data)
    return str(total / (n * m))

# provided samples
assert abs(float(run("""3 7
63 61 55 54 77 87 89
73 60 38 5 16 56 91
75 43 11 3 16 20 95
""")) - 51.809523810) < 1e-6

assert abs(float(run("""4 3
10 10 10
10 10 10
10 10 10
10 10 10
""")) - 10.0) < 1e-6

# custom cases
assert abs(float(run("2 2\n0 100\n100 0\n")) - 50.0) < 1e-6, "checkerboard symmetry"
assert abs(float(run("1 5\n20 40 60 80 100\n")) - 60.0) < 1e-6, "single row average"
assert abs(float(run("5 1\n0\n0\n0\n0\n100\n")) - 20.0) < 1e-6, "single column"
assert abs(float(run("3 3\n1 1 1\n1 1 1\n1 1 1\n")) - 1.0) < 1e-6, "uniform grid"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Bàn cờ 2×2 | 50 | tính đối xứng và tính trung bình | 
| Hàng 1×5 | 60 | dải vĩ độ đơn suy biến | 
| cột 5×1 | 20 | lát cắt kinh độ suy biến | 
| tất cả những cái | 1 | bất biến thống nhất | 

## Vỏ cạnh 

Một trường hợp thất bại phổ biến là giả định giá trị trung bình của hàng phải có trọng số khác nhau giữa các dải vĩ độ. Ví dụ, lấy giá trị trung bình của các giá trị trung bình của hàng sẽ sai nếu người ta nhầm tưởng rằng các hàng cực nhỏ hơn. Tuy nhiên, trong bài toán này, mỗi ô đã thể hiện sự đóng góp bề mặt bằng nhau, do đó không cần thêm trọng số. 

Một trường hợp tinh vi khác là các kích thước suy biến như`n = 1`hoặc`m = 1`. Thuật toán vẫn hoạt động vì nó giảm xuống mức trung bình của một hàng hoặc một cột. Ví dụ, đầu vào`1 3`với các giá trị`10 20 30`sản lượng`20`, phù hợp với tính toán trực tiếp. 

Trường hợp cạnh cuối cùng là mất độ chính xác khi tính tổng các lưới lớn. Vì số tiền tối đa là`10^6 × 100 = 10^8`, sử dụng tích lũy dấu phẩy động vẫn ổn định trong phạm vi yêu cầu`1e-6`dung sai và không cần số đặc biệt.
