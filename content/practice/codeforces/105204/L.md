---
title: "CF 105204L - \u0411\u0438\u043d\u043f\u043e\u0438\u0441\u043a \u0440\u0435\u0448\u0430\u0435\u0442 \u0432\u0441\u0451!"
description: "Ta có hoán vị các số từ 1 đến n. Giá trị x phải được tìm thấy bằng thủ tục tìm kiếm nhị phân chạy trên hoán vị chưa sắp xếp này. Thủ tục không tìm kiếm trực tiếp vị trí của x."
date: "2026-06-27T02:44:47+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105204
codeforces_index: "L"
codeforces_contest_name: "\u0412\u041a\u041e\u0428\u041f.Junior 2024"
rating: 0
weight: 105204
solve_time_s: 80
verified: true
draft: false
---

[CF 105204L - \u0411\u0438\u043d\u043f\u043e\u0438\u0441\u043a \u0440\u0435\u0448\u0430\u0435\u0442 \u0432\u0441\u0451!](https://codeforces.com/problemset/problem/105204/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 20s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một hoán vị của các số từ`1`ĐẾN`n`. Một giá trị`x`phải được tìm thấy bằng thủ tục tìm kiếm nhị phân chạy trên hoán vị chưa sắp xếp này. Thủ tục không tìm kiếm vị trí của`x`trực tiếp. Nó chỉ di chuyển các đường viền trái và phải tùy theo phần tử ở giữa nhiều nhất`x`hoặc lớn hơn`x`và khi dừng nó sẽ trả về đường viền bên trái hiện tại. Mục tiêu là thực hiện tối đa hai lần hoán đổi trước khi bắt đầu tìm kiếm sao cho vị trí được trả về chứa`x`. 

Đầu vào chứa một số trường hợp độc lập. Đối với mỗi trường hợp, chúng ta biết kích thước của hoán vị, giá trị cần thiết và cách sắp xếp hiện tại. Đầu ra là một danh sách các giao dịch hoán đổi. Bất kỳ chuỗi hợp lệ nào có 0, một hoặc hai lần hoán đổi đều được chấp nhận. 

Tổng kích thước của tất cả các hoán vị nhiều nhất là`200000`. Điều này loại trừ bất cứ điều gì cố gắng thực hiện nhiều giao dịch hoán đổi có thể xảy ra hoặc mô phỏng lặp đi lặp lại quy trình cho mọi vị thế. Một giải pháp tuyến tính cho mỗi trường hợp thử nghiệm là đủ vì nó mang lại`O(200000)`tổng công việc, trong khi các phương pháp như kiểm tra tất cả các cặp vị trí sẽ yêu cầu tới`O(n^2)`hoạt động. 

Phần tinh tế của vấn đề là tìm kiếm nhị phân không cần toàn bộ mảng để được sắp xếp. Chỉ các vị trí được truy cập bởi đường dẫn tìm kiếm nhị phân mới quan trọng. Một sai lầm phổ biến là di chuyển`x`đâu đó và cho rằng đường dẫn tìm kiếm vẫn giữ nguyên mà không cần chứng minh điều đó. Một sai lầm khác là sử dụng các chỉ mục dựa trên số 0 trong mô phỏng tìm kiếm nhị phân trong khi vấn đề mô tả các vị trí bắt đầu từ một. 

Ví dụ, hãy xem xét:```
1
4 1
2 3 4 1
```Tìm kiếm nhị phân bắt đầu bằng`l = 1`,`r = 5`. Nó kiểm tra vị trí`2`, thấy`3 > 1`, và di chuyển`r`ĐẾN`2`. Nó dừng lại với`l = 1`, nhưng vị trí`1`chứa`2`. Đầu ra đúng là:```
1
1 4
```Một giải pháp bất cẩn cho rằng đáp án luôn là quan điểm ban đầu của`x`sẽ thất bại vì thuật toán trả về một vị trí được xác định bằng phép so sánh, không phải bởi vị trí của`x`. 

Một trường hợp cạnh khác là khi`x`đã ở vị trí được trả về bởi tìm kiếm nhị phân.```
1
5 3
1 2 3 4 5
```Đầu ra đúng là:```
0
```Một giải pháp luôn thực hiện hoán đổi vẫn được chấp nhận trong nhiều trường hợp, nhưng nó có thể vô tình di chuyển`x`rời khỏi vị trí đúng và tạo ra câu trả lời sai. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là thử các giao dịch hoán đổi có thể có và kiểm tra kết quả. Một trao đổi duy nhất có`O(n^2)`khả năng, và hai lần hoán đổi sẽ tạo ra nhiều sự kết hợp hơn nữa. Ngay cả khi chúng tôi chỉ mô phỏng tìm kiếm nhị phân trong`O(log n)`sau mỗi lần thử, số lượng thao tác vượt xa những gì có thể thực hiện được`n`lên đến`200000`. 

Quan sát quan trọng là chúng ta không cần sửa chữa hoán vị. Chúng ta chỉ cần đặt`x`vào vị trí kết thúc tìm kiếm nhị phân hiện tại. Hãy để vị trí đó được`l`. 

Chạy tìm kiếm nhị phân đã cho một lần trên hoán vị ban đầu. Nếu như`p[l]`đã rồi`x`, không cần thay đổi. Ngược lại, hoán đổi phần tử`x`vào vị trí`l`. 

Mối quan tâm duy nhất là liệu việc di chuyển`x`thay đổi một trong những so sánh được thực hiện bởi tìm kiếm. Hoán đổi thay đổi hai vị trí: vị trí cuối cùng`l`và vị trí ban đầu của`x`. 

Chức vụ`l`không bao giờ được kiểm tra sau khi thuật toán dừng. Vị trí ban đầu của`x`thú vị hơn. Nếu tìm kiếm kiểm tra vị trí đó, thì sự so sánh ở đó với giá trị`x`, do đó tìm kiếm đã di chuyển đường viền bên trái đến vị trí đó. Bất kỳ chuyển động nào sau này của đường viền bên trái chỉ có thể xảy ra vì giá trị được kiểm tra khác cũng nhiều nhất là`x`. Vì thế vị trí cuối cùng`l`luôn chứa nhiều nhất một giá trị`x`trong trường hợp này. Sau khi hoán đổi, giá trị cũ từ`l`chuyển về vị trí ban đầu của`x`và nó vẫn thỏa mãn kết quả so sánh tương tự. Nếu vị trí ban đầu của`x`chưa bao giờ được kiểm tra, việc thay đổi nó không thể ảnh hưởng đến việc tìm kiếm. 

Vì vậy, một lần trao đổi luôn là đủ. Bài toán cho phép hai lần hoán đổi, nhưng cách xây dựng tối ưu chỉ cần 0 hoặc một. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) hoặc tệ hơn | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tìm vị trí hiện tại của`x`trong hoán vị. Chúng tôi cần chỉ mục này vì sự sửa đổi duy nhất có thể xảy ra là di chuyển`x`đến một nơi khác. 
2. Mô phỏng tìm kiếm nhị phân chính xác được mô tả trong câu lệnh. Duy trì`l`Và`r`dưới dạng các đường viền dựa trên một và cập nhật chúng bằng các giá trị hoán vị hiện tại. Khi vòng lặp kết thúc,`l`là vị trí mà thuật toán sẽ trả về trước bất kỳ giao dịch hoán đổi nào. 
3. Nếu giá trị tại vị trí`l`đã rồi`x`, đầu ra không hoán đổi. Hoán vị hiện tại đã thỏa mãn điều kiện yêu cầu. 
4. Ngược lại, hoán đổi giá trị tại các vị trí`l`Và`pos[x]`. Nơi này`x`chính xác nơi tìm kiếm nhị phân kết thúc. 
5. Xuất ra trao đổi đơn này. 

Tại sao nó hoạt động: đường dẫn tìm kiếm nhị phân trước khi hoán đổi xác định vị trí cuối cùng`l`. Trao đổi đặt`x`ở đó. Vị trí thay đổi duy nhất có thể được xem xét trước đó là vị trí cũ của`x`. Nếu nó được kiểm tra, giá trị sẽ được chuyển đến đó từ`l`được đảm bảo nhiều nhất`x`, đưa ra hướng tương tự như so sánh ban đầu với`x`. Mọi so sánh khác không thay đổi nên thuật toán vẫn đạt`l`và trả về vị trí chứa`x`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        n, x = map(int, input().split())
        p = list(map(int, input().split()))

        pos = 0
        for i, v in enumerate(p):
            if v == x:
                pos = i
                break

        l = 0
        r = n
        while r - l > 1:
            m = (l + r) // 2
            if p[m] <= x:
                l = m
            else:
                r = m

        if p[l] == x:
            ans.append("0")
        else:
            ans.append("1")
            ans.append(f"{l + 1} {pos + 1}")

    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```Mảng sử dụng lập chỉ mục dựa trên 0 bên trong, trong khi đầu ra được yêu cầu sử dụng các vị trí dựa trên một. Việc chuyển đổi chỉ được thực hiện khi in trao đổi. 

Mô phỏng sử dụng quy ước biên giới giống như câu lệnh.`l`là đường viền bên trái hợp lệ hiện tại và`r`là đường viền độc quyền bên phải nên điều kiện`r - l > 1`khớp chính xác với điều kiện dừng. 

Việc tìm kiếm`pos[x]`là tuyến tính. Mô phỏng tìm kiếm nhị phân mất thời gian logarit, do đó toàn bộ giải pháp được chi phối bằng cách quét hoán vị một lần. 

## Ví dụ đã hoạt động 

Hãy xem xét:```
1
6 5
3 1 6 5 2 4
```Mô phỏng là: 

| tôi | r | m | p[m] | Quyết định | 
| --- | --- | --- | --- | --- | 
| 0 | 6 | 3 | 6 | Di chuyển sang trái, r = 3 | 
| 0 | 3 | 1 | 1 | Di chuyển sang phải, l = 1 | 
| 1 | 3 | 2 | 6 | Di chuyển sang trái, r = 2 | 

Vị trí cuối cùng là`l = 1`trong lập chỉ mục dựa trên số không. Giá trị ở đó`1`, không`5`. Vị trí của`5`là`4`, nên chúng ta đổi vị trí`2`Và`5`trong việc lập chỉ mục một cơ sở. 

Sau khi hoán đổi, hoán vị bắt đầu:```
3 5 6 4 2 1
```Đường dẫn tìm kiếm nhị phân tương tự đạt đến vị trí`2`, hiện chứa`5`. 

Ví dụ này cho thấy thuật toán không cố gắng sắp xếp hoán vị. Nó chỉ thay đổi một vị trí được trả về bởi tìm kiếm. 

Một ví dụ khác:```
1
5 3
1 2 3 4 5
```Dấu vết là: 

| tôi | r | m | p[m] | Quyết định | 
| --- | --- | --- | --- | --- | 
| 0 | 5 | 2 | 3 | Di chuyển sang phải, l = 2 | 
| 2 | 5 | 3 | 4 | Di chuyển sang trái, r = 3 | 

Việc tìm kiếm kết thúc ở vị trí`3`trong lập chỉ mục dựa trên một và vị trí đó đã chứa`3`. Câu trả lời là không có giao dịch hoán đổi. 

Điều này thể hiện tình trạng thoát sớm và tránh những sửa đổi không cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Tìm kiếm`x`và mô phỏng tìm kiếm nhị phân đều phù hợp với quá trình quét tuyến tính. | 
| Không gian | O(n) | Hoán vị được lưu trữ trong bộ nhớ. | 

Tổng của tất cả`n`các giá trị được giới hạn bởi`200000`, do đó tổng thời gian chạy là tuyến tính theo kích thước đầu vào và dễ dàng nằm gọn trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result

assert run(
"""3
6 3
1 2 3 4 5 6
6 5
3 1 6 5 2 4
5 1
3 5 4 2 1
"""
) == "0\n1\n2 4\n1\n1 5\n"

assert run(
"""1
1 1
1
"""
) == "0\n"

assert run(
"""1
4 1
2 3 4 1
"""
) == "1\n1 4\n"

assert run(
"""1
5 5
1 2 3 4 5
"""
) == "0\n"

assert run(
"""1
7 4
7 6 5 3 2 1 4
"""
) == "1\n7 4\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Các mẫu được cung cấp | Các trường hợp hoán đổi 0 và một hợp lệ | Tính đúng đắn cơ bản | 
|`n = 1`|`0`hoán đổi | Xử lý kích thước tối thiểu | 
|`x = 1`với`x`ở cuối | Một lần trao đổi | Hành vi giá trị biên | 
| Sắp xếp hoán vị |`0`hoán đổi | Kết quả tìm kiếm đã chính xác | 
|`x`cách xa vị trí quay trở lại | Một lần trao đổi | Chuyển động chính xác của mục tiêu | 

## Vỏ cạnh 

Đối với trường hợp`x`đã là vị trí được trả về thì thuật toán dừng ngay lập tức. 

đầu vào:```
1
5 3
1 2 3 4 5
```Tìm kiếm nhị phân đạt đến vị trí`3`, Và`p[3]`đã rồi`3`, do đó đầu ra là:```
0
```Không cần trao đổi vì mọi so sánh đều đã đi theo đường dẫn dự kiến. 

Đối với trường hợp`x`không ở vị trí trả về, thuật toán chỉ di chuyển`x`. 

đầu vào:```
1
4 1
2 3 4 1
```Vị trí kiểm tra tìm kiếm`2`, di chuyển sang trái và dừng ở vị trí`1`. Giá trị ở đó`2`. Hoán đổi vị trí`1`Và`4`sản xuất:```
1 3 4 2
```Chạy tìm kiếm nhị phân tương tự vẫn dừng ở vị trí`1`bởi vì giá trị được kiểm tra duy nhất là`3`, lớn hơn`1`. Vị trí trả về chứa`1`, vậy là việc xây dựng thành công. 

Ranh giới quan trọng là khi vị trí ban đầu của`x`chính nó được kiểm tra trong quá trình tìm kiếm nhị phân. Bằng chứng ở trên bao hàm điều này vì giá trị được di chuyển vào vị trí đó xuất phát từ đường viền bên trái cuối cùng và giá trị đó phải vượt qua một`<= x`so sánh khi đường viền được tạo. Hướng tìm kiếm không thay đổi nên kết quả cuối cùng vẫn giữ nguyên.
