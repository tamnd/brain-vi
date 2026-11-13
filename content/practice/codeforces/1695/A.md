---
title: "CF 1695A - Đoán hình chữ nhật"
description: "Chúng ta được cung cấp một lưới chứa các số nguyên riêng biệt. Michael chọn kích thước h × w, sau đó Joe bí mật chọn bất kỳ hình chữ nhật con nào có kích thước chính xác như vậy. Michael phải đặt tên cho giá trị lớn nhất bên trong hình chữ nhật đã chọn của Joe trước khi xem hình chữ nhật nào đã được chọn."
date: "2026-06-09T22:42:59+07:00"
tags: ["codeforces", "competitive-programming", "games"]
categories: ["algorithms"]
codeforces_contest: 1695
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 801 (Div. 2) and EPIC Institute of Technology Round"
rating: 800
weight: 1695
solve_time_s: 142
verified: true
draft: false
---

[CF 1695A - Đoán hình chữ nhật phụ](https://codeforces.com/problemset/problem/1695/A) 

**Đánh giá:** 800 
**Thẻ:** trò chơi 
**Thời gian giải:** 2m 22s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một lưới chứa các số nguyên riêng biệt. Michael chọn kích thước`h × w`, sau đó Joe bí mật chọn bất kỳ hình chữ nhật con nào có kích thước chính xác như vậy. Michael phải đặt tên cho giá trị lớn nhất bên trong hình chữ nhật đã chọn của Joe trước khi xem hình chữ nhật nào đã được chọn. 

Vì tất cả các giá trị đều khác biệt nên toàn bộ bảng có mức tối đa toàn cầu duy nhất. Michael chỉ có thể đảm bảo chiến thắng nếu có thể`h × w`hình chữ nhật con chứa cùng một ô đó. Trong trường hợp đó, bất kể lựa chọn của Joe là gì, mức tối đa bên trong hình chữ nhật đã chọn luôn là mức tối đa chung của toàn bộ bảng. 

Nhiệm vụ là tìm diện tích nhỏ nhất có thể`h · w`mà sự đảm bảo này có thể thực hiện được. 

Kích thước thỏa mãn`n, m ≤ 40`, nó rất nhỏ. Ngay cả các thuật toán thử tất cả các kích thước hình chữ nhật có thể cũng dễ dàng có giá phải chăng. chỉ có`40 × 40 = 1600`khả thi`(h, w)`cặp cho mỗi trường hợp thử nghiệm, vì vậy một giải pháp toán học trực tiếp là quá đủ. 

Một điểm tinh tế là Michael không cần biết Joe đã chọn hình chữ nhật con nào. Anh ta chỉ cần một con số đảm bảo là lớn nhất trong mọi lựa chọn có thể. Bởi vì tất cả các giá trị đều khác biệt nên ứng cử viên duy nhất là mức tối đa toàn cầu của toàn bộ bảng. 

Hãy xem xét lưới này:```
1 2
3 4
```Giá trị tối đa`4`nằm ở góc dưới bên phải. Lựa chọn`h = 1, w = 1`thất bại vì Joe có thể chọn ô chứa`1`, có mức tối đa không phải là`4`. 

Câu trả lời đúng là`4`, thu được bằng cách chọn toàn bộ bảng. Một cách tiếp cận bất cẩn chỉ kiểm tra xem một số hình chữ nhật có chứa giá trị tối đa hay không sẽ trả về sai`1`. 

Một sai lầm dễ mắc phải khác là quên rằng mức tối đa có thể nằm trên một cạnh.```
5 1 2
3 4 0
```Tối đa là ở vị trí`(1,1)`. Câu trả lời không được xác định bởi khoảng cách đến trung tâm. Kích thước hình chữ nhật phải đủ lớn để mọi vị trí có thể đều bao gồm góc trên cùng bên trái. 

## Phương pháp tiếp cận 

Quan điểm vũ phu bắt đầu bằng cách xác định ô chứa mức tối đa toàn cục. Giả sử ô đó nằm ở hàng`r`và cột`c`sử dụng chỉ mục dựa trên 1. 

Đối với mỗi cặp`(h, w)`, chúng ta có thể liệt kê tất cả những gì có thể`h × w`hình chữ nhật con và kiểm tra xem mỗi hình chữ nhật có chứa`(r, c)`. Nếu có, Michael thắng với lựa chọn đó. Sau đó chúng tôi giữ diện tích tối thiểu. 

Điều này đúng vì Michael thắng chính xác khi mọi hình chữ nhật hợp lệ đều chứa ô tối đa toàn cục. Vấn đề là với mọi kích thước, chúng ta sẽ kiểm tra nhiều vị trí hình chữ nhật. Mặc dù các ràng buộc đủ nhỏ để điều này vẫn vượt qua nhưng nó che giấu cấu trúc thực sự của vấn đề. 

Quan sát quan trọng là chúng ta không cần liệt kê các hình chữ nhật. 

Đối với chiều cao cố định`h`, hàng bắt đầu của hình chữ nhật có thể có giá trị bất kỳ từ`1`ĐẾN`n - h + 1`. 

Đối với mỗi hình chữ nhật như vậy có chứa hàng`r`, hai điều kiện phải xảy ra đồng thời:```
start_row ≤ r
start_row + h - 1 ≥ r
```Các hàng bắt đầu hạn chế nhất là hàng nhỏ nhất có thể và hàng lớn nhất có thể. 

Sự khởi đầu nhỏ nhất là`1`, đòi hỏi:```
1 + h - 1 ≥ r
h ≥ r
```Sự khởi đầu lớn nhất là`n - h + 1`, đòi hỏi:```
n - h + 1 ≤ r
h ≥ n - r + 1
```Kết hợp chúng mang lại:```
h ≥ max(r, n - r + 1)
```Chính xác lý do tương tự cho các cột đưa ra:```
w ≥ max(c, m - c + 1)
```Để giảm thiểu diện tích, chúng tôi chọn chiều cao và chiều rộng hợp lệ nhỏ nhất:```
h = max(r, n - r + 1)
w = max(c, m - c + 1)
```Câu trả lời đơn giản là`h · w`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²m²) | O(1) | Về mặt khái niệm quá chậm, không cần thiết | 
| Tối ưu | O(nm) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Quét lưới và xác định vị trí ô chứa giá trị lớn nhất toàn cục. 
2. Đặt vị trí của nó là`(r, c)`trong lập chỉ mục dựa trên 1. 
3. Tính chiều cao tối thiểu buộc mọi hình chữ nhật hợp lệ phải chứa hàng`r`:```
h = max(r, n - r + 1)
```Đây là chiều cao nhỏ nhất mà cả vị trí trên cùng và dưới cùng đều không thể tránh được hàng`r`. 
4. Tính chiều rộng tối thiểu buộc mọi hình chữ nhật hợp lệ phải chứa cột`c`:```
w = max(c, m - c + 1)
```5. Đầu ra:```
h × w
```### Tại sao nó hoạt động 

Mỗi`h × w`hình chữ nhật chứa ô tối đa khi và chỉ khi mỗi khoảng độ dài hàng hợp lệ`h`chứa hàng`r`và mỗi khoảng cột pháp lý có độ dài`w`chứa cột`c`. 

Độ cao nhỏ nhất thỏa mãn tính chất này là`max(r, n-r+1)`. Bất kỳ chiều cao nhỏ hơn nào cũng cho phép vị trí trên cùng hoặc vị trí dưới cùng bỏ qua hàng`r`. Đối số tương tự áp dụng cho chiều rộng. 

Vì chiều cao và chiều rộng độc lập nên việc chọn giá trị hợp lệ tối thiểu cho từng kích thước sẽ giảm thiểu diện tích. Mỗi hình chữ nhật thu được đều chứa ô tối đa toàn cục, vì vậy giá trị tối đa của nó luôn giống nhau. Michael luôn có thể trả lời đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())

for _ in range(t):
    n, m = map(int, input().split())

    mx = -10**18
    r = c = 0

    for i in range(1, n + 1):
        row = list(map(int, input().split()))
        for j in range(1, m + 1):
            if row[j - 1] > mx:
                mx = row[j - 1]
                r, c = i, j

    h = max(r, n - r + 1)
    w = max(c, m - c + 1)

    print(h * w)
```Vòng lặp đầu tiên đọc lưới đồng thời định vị mức tối đa toàn cầu. Vì tất cả các giá trị đều khác nhau nên ô tối đa là duy nhất. 

Sau khi tìm được vị trí của nó`(r, c)`, công thức tính chiều cao và chiều rộng cần thiết tuân theo trực tiếp từ đối số hình học được phát triển ở trên. 

Việc sử dụng các chỉ số dựa trên 1 sẽ đơn giản hóa các công thức. số lượng`n - r + 1`Và`m - c + 1`biểu thị khoảng cách đến các đường biên đối diện được đo một cách toàn diện. Lấy mức tối đa sẽ chọn phía xa hơn, điều này xác định độ lớn của hình chữ nhật là không thể tránh khỏi. 

Không cần thêm mảng nào ngoài việc đọc từng hàng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 3
-7 5 2
0 8 -3
```Giá trị tối đa là`8`. 

| Biến | Giá trị | 
| --- | --- | 
| n | 2 | 
| m | 3 | 
| r | 2 | 
| c | 2 | 
| h | tối đa(2, 1) = 2 | 
| w | tối đa(2, 2) = 2 | 
| Trả lời | 4 | 

Diện tích kết quả là`2 × 2 = 4`. 

Bất kì`2 × 2`hình chữ nhật con phải bao gồm hàng 2 và cột 2, do đó nó phải chứa ô có giá trị`8`. 

### Ví dụ 2 

đầu vào:```
4 4
2 12 6 10
3 15 16 4
1 13 8 11
14 7 9 5
```Giá trị tối đa là`16`. 

| Biến | Giá trị | 
| --- | --- | 
| n | 4 | 
| m | 4 | 
| r | 2 | 
| c | 3 | 
| h | tối đa(2, 3) = 3 | 
| w | tối đa(3, 2) = 3 | 
| Trả lời | 9 | 

Câu trả lời là`3 × 3 = 9`. 

Ví dụ này chứng minh rằng hình chữ nhật tối ưu không nhất thiết phải là toàn bộ bảng. Nó chỉ cần đủ lớn để mọi vị trí đều giao nhau với ô tối đa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nm) | Một lần quét lưới để xác định vị trí tối đa | 
| Không gian | O(1) | Chỉ có một vài biến được lưu trữ | 

Với`n, m ≤ 40`, tối đa 1600 ô được xử lý cho mỗi trường hợp thử nghiệm. Giải pháp chạy thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    t = int(input())
    ans = []

    for _ in range(t):
        n, m = map(int, input().split())

        mx = -10**18
        r = c = 0

        for i in range(1, n + 1):
            row = list(map(int, input().split()))
            for j in range(1, m + 1):
                if row[j - 1] > mx:
                    mx = row[j - 1]
                    r, c = i, j

        ans.append(str(max(r, n - r + 1) *
                       max(c, m - c + 1)))

    print("\n".join(ans))

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    old_stdout = sys.stdout
    sys.stdout = out
    solve()
    sys.stdout = old_stdout
    return out.getvalue()

# provided sample
assert run("""3
1 1
3
4 4
2 12 6 10
3 15 16 4
1 13 8 11
14 7 9 5
2 3
-7 5 2
0 8 -3
""") == "1\n9\n4\n"

# minimum size
assert run("""1
1 1
42
""") == "1\n"

# maximum in top-left corner
assert run("""1
2 2
10 1
2 3
""") == "4\n"

# maximum in center of 3x3
assert run("""1
3 3
1 2 3
4 9 5
6 7 8
""") == "4\n"

# maximum in bottom-right corner
assert run("""1
3 4
1 2 3 4
5 6 7 8
9 10 11 12
""") == "12\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Lưới 1×1 | 1 | Bảng nhỏ nhất có thể | 
| Tối đa ở trên cùng bên trái | 4 | Xử lý vị trí biên giới | 
| Tối đa ở giữa 3×3 | 4 | Nội thất tối đa | 
| Tối đa ở dưới cùng bên phải | 12 | Góc đối diện và khoảng cách bao gồm | 

## Vỏ cạnh 

Hãy xem xét một bảng trong đó mức tối đa nằm ở một góc:```
1
2 2
10 1
2 3
```Tối đa là ở`(1,1)`. 

Thuật toán tính toán:```
h = max(1, 2) = 2
w = max(1, 2) = 2
answer = 4
```Bất kỳ kích thước nhỏ hơn nào cũng sẽ cho phép một hình chữ nhật tránh hoàn toàn góc. Toàn bộ bảng là cần thiết. 

Bây giờ hãy xem xét mức tối đa ở giữa:```
1
3 3
1 2 3
4 9 5
6 7 8
```Tối đa là ở`(2,2)`. 

Thuật toán tính toán:```
h = max(2, 2) = 2
w = max(2, 2) = 2
answer = 4
```Mọi`2 × 2`hình chữ nhật của một`3 × 3`bảng chứa ô trung tâm. Một sai lầm phổ biến là cho rằng câu trả lời phải là`3 × 3 = 9`, nhưng trung tâm đã trở nên không thể tránh khỏi với diện tích`4`. 

Cuối cùng, hãy xem xét một hàng duy nhất:```
1
1 5
1 2 10 3 4
```Tối đa là ở cột`3`. 

Thuật toán đưa ra:```
h = 1
w = max(3, 3) = 3
answer = 3
```Mọi đoạn liền kề có độ dài 3 trong hàng có độ dài 5 đều chứa vị trí ở giữa. Công thức xử lý bảng một chiều một cách tự nhiên, không có trường hợp đặc biệt nào.
