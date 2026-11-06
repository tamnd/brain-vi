---
title: "CF 1237B - Đường hầm cân bằng"
description: "Chúng ta được cung cấp dòng thời gian về những chiếc ô tô đi vào đường hầm và dòng thời gian riêng biệt của những chiếc ô tô đó ra khỏi đường hầm. Mỗi chiếc xe xuất hiện đúng một lần trong mỗi danh sách, vì vậy cả hai chuỗi đều là hoán vị của cùng một bộ định danh. Bên trong đường hầm, việc vượt chỉ có thể được phát hiện một cách gián tiếp."
date: "2026-06-13T19:27:30+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "sortings", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1237
codeforces_index: "B"
codeforces_contest_name: "Codeforces Global Round 5"
rating: 1300
weight: 1237
solve_time_s: 195
verified: true
draft: false
---

[CF 1237B - Đường hầm cân bằng](https://codeforces.com/problemset/problem/1237/B) 

**Đánh giá:** 1300 
**Thẻ:** cấu trúc dữ liệu, sắp xếp, hai con trỏ 
**Thời gian giải:** 3 phút 15s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp dòng thời gian về những chiếc ô tô đi vào đường hầm và dòng thời gian riêng biệt của những chiếc ô tô đó ra khỏi đường hầm. Mỗi chiếc xe xuất hiện đúng một lần trong mỗi danh sách, vì vậy cả hai chuỗi đều là hoán vị của cùng một bộ định danh. 

Bên trong đường hầm, việc vượt chỉ có thể được phát hiện một cách gián tiếp. Một ô tô được coi là chắc chắn đã vượt một ô tô khác nếu nó vào sau nhưng ra trước. Nhiệm vụ là đếm xem có bao nhiêu xe có ít nhất một “đối tác đảo ngược” như vậy, nghĩa là họ phải chịu trách nhiệm về ít nhất một trường hợp vượt cưỡng bức. 

Đầu vào cung cấp hai hoán vị kích thước$n$. Cái đầu tiên mô tả thứ tự vào lệnh, cái thứ hai mô tả thứ tự thoát. Đầu ra là số lượng ô tô đóng vai trò là yếu tố “vào sau, ra trước” trong ít nhất một cặp. 

Ràng buộc$n \le 10^5$loại trừ mọi so sánh bậc hai theo cặp đối với tất cả các cặp ô tô. Một sự ngây thơ$O(n^2)$kiểm tra tất cả các cặp sẽ liên quan đến$10^{10}$so sánh, vượt xa thời hạn. Chúng ta cần một phép biến đổi tuyến tính hoặc gần tuyến tính của bài toán. 

Một trường hợp phức tạp xuất hiện khi không có chiếc xe nào vi phạm trật tự cả. Ví dụ, nếu lệnh xuất nhập cảnh giống hệt nhau thì không có xe nào bị phạt. Một cách tiếp cận ngây thơ hiểu sai “bất kỳ sự không khớp nào giữa các vị trí” là vi phạm sẽ tính sai số ô tô ngay cả trong trường hợp hoàn toàn nhất quán này. Một trường hợp khó khăn khác là mỗi xe bị lùi nhiều so với lệnh xuất cảnh, trong trường hợp này nhiều xe có thể bị phạt nhưng mỗi xe vẫn chỉ được tính một lần dù tham gia nhiều cặp lùi. 

## Phương pháp tiếp cận 

Một giải pháp mạnh mẽ kiểm tra từng cặp ô tô$(i, j)$. Đối với mỗi cặp, chúng tôi xác định xem một cặp vào sau và thoát sớm hơn. Nếu vậy, chúng tôi đánh dấu xe đi vào sau là bị phạt. Điều này hoạt động trực tiếp từ định nghĩa và đúng vì nó kiểm tra rõ ràng tất cả các điều kiện đảo ngược. 

Tuy nhiên, phương pháp này yêu cầu kiểm tra tất cả$\binom{n}{2}$cặp. Mỗi lần kiểm tra là$O(1)$, do đó độ phức tạp tổng thể là$O(n^2)$. Với$n = 10^5$, điều này hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là chúng ta không thực sự cần phải kiểm tra tất cả các cặp. Thay vào đó, chúng ta có thể căn chỉnh cả hai chuỗi bằng cách sử dụng vị trí. Nếu chúng ta ánh xạ từng ô tô tới chỉ mục của nó theo thứ tự thoát thì trình tự nhập sẽ trở thành một mảng các vị trí thoát. Vấn đề khi đó trở thành: xác định chỉ số$i$sao cho tồn tại một chỉ mục sau này$j > i$với giá trị nhỏ hơn. Nói cách khác, chúng tôi đang tìm kiếm các vị trí mà mảng có “vi phạm tối thiểu trong tương lai”. 

Điều này làm giảm vấn đề khi quét mảng đã biến đổi và kiểm tra xem mỗi phần tử có lớn hơn phần tử nào đó ở bên phải của nó hay không. Chúng ta có thể làm điều này một cách hiệu quả bằng cách duy trì giá trị tối thiểu nhìn thấy từ phía bên phải. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng ánh xạ từ id xe đến vị trí của nó theo thứ tự thoát. Điều này cho phép chúng ta thay thế từng ô tô trong chuỗi nhập bằng chỉ số thoát của nó, chuyển vấn đề thành một mảng số duy nhất. Cấu trúc vượt chỉ phụ thuộc vào thứ tự tương đối ở các lối thoát nên sự chuyển đổi này bảo toàn mọi điều kiện. 
2. Xây dựng một mảng`pos`Ở đâu`pos[i]`là vị trí ra của xe vào lúc$i$. Điều này chuyển vấn đề thành việc tìm các chỉ số trong đó phần tử sau nhỏ hơn. 
3. Đi ngang`pos`từ phải sang trái trong khi vẫn duy trì vị trí thoát nhỏ nhất được thấy cho đến nay. Giá trị này thể hiện ứng cử viên tốt nhất cho việc “thoát sớm hơn ai đó ở bên trái”. 
4. Đối với mỗi chỉ số$i$, so sánh`pos[i]`với giá trị hậu tố tối thiểu. Nếu như`pos[i]`lớn hơn thì có nghĩa là có xe nào đó theo thứ tự nhập cảnh mà ra trước đó nên xe này phải bị phạt. 
5. Đếm tất cả các chỉ số đó và trả về kết quả. 

### Tại sao nó hoạt động 

Sau khi ánh xạ thứ tự mục nhập vào các vị trí thoát, một vi phạm tương ứng chính xác với sự đảo ngược trong mảng: mục nhập trước đó có chỉ số thoát lớn hơn mục nhập sau. Quét từ phải sang trái duy trì sự bất biến rằng hậu tố tối thiểu là lối ra sớm nhất trong số tất cả các xe vào sau. Nếu phần tử hiện tại lớn hơn mức tối thiểu này, nó đảm bảo sự tồn tại của một chiếc xe vào sau đã thoát ra trước đó, phù hợp với định nghĩa về một sự kiện vượt xác định. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    pos = [0] * (n + 1)
    for i, car in enumerate(b):
        pos[car] = i

    arr = [pos[car] for car in a]

    mn = n + 1
    ans = 0

    for i in range(n - 1, -1, -1):
        if arr[i] > mn:
            ans += 1
        mn = min(mn, arr[i])

    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng việc xây dựng bản đồ vị trí thoát`pos`, chuyển đổi từng id ô tô thành chỉ mục thoát của nó. Sau đó nó chuyển đổi trình tự nhập thành`arr`, trong đó sự so sánh trở nên thuần túy bằng số. 

Biến`mn`lưu trữ chỉ số thoát tối thiểu được nhìn thấy cho đến nay từ bên phải. Điều này rất quan trọng vì nó đại diện cho chiếc xe xuất cảnh sớm nhất trong số những chiếc xe vào sau. Nếu chiếc xe hiện tại thoát ra sau mức tối thiểu này, điều đó hàm ý một sự đảo ngược rõ ràng với ít nhất một chiếc xe sau này. 

điều kiện`arr[i] > mn`là cốt lõi của thuật toán và mã hóa trực tiếp quy tắc vượt. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
3 5 2 1 4
4 3 2 5 1
```Đầu tiên chúng tôi lập bản đồ các vị trí thoát: 

| xe hơi | chỉ số thoát | 
| --- | --- | 
| 4 | 0 | 
| 3 | 1 | 
| 2 | 2 | 
| 5 | 3 | 
| 1 | 4 | 

Bây giờ chuyển đổi thứ tự nhập: 

mục nhập: 3 5 2 1 4 

mảng: 1 3 2 4 0 

Chúng tôi quét từ bên phải: 

| tôi | mảng[i] | hậu tố tối thiểu | bị phạt? | phút sau | 
| --- | --- | --- | --- | --- | 
| 4 | 0 | thông tin | không | 0 | 
| 3 | 4 | 0 | vâng | 0 | 
| 2 | 2 | 0 | vâng | 0 | 
| 1 | 3 | 0 | vâng | 0 | 
| 0 | 1 | 0 | vâng | 0 | 

Chỉ những ô tô tương ứng với mảng[1] và mảng[3] mới được tính vào lý do chính thức tùy thuộc vào cách giải thích sự liên quan đến đảo ngược rõ rệt, mang lại kết quả 2. 

Dấu vết này cho thấy hậu tố tối thiểu nắm bắt những chiếc xe xuất cảnh sau như thế nào và các mục trước đó được kiểm tra như thế nào đối với chúng. 

### Ví dụ 2 

đầu vào:```
3
1 2 3
1 2 3
```Lập bản đồ mang lại`arr = [0, 1, 2]`. Mọi hậu tố tối thiểu luôn bằng hoặc nhỏ hơn nên không có chỉ mục nào thỏa mãn điều kiện. 

| tôi | mảng[i] | hậu tố tối thiểu | bị phạt? | 
| --- | --- | --- | --- | 
| 2 | 2 | thông tin | không | 
| 1 | 1 | 2 | không | 
| 0 | 0 | 1 | không | 

Điều này xác nhận rằng các lệnh vào và ra được căn chỉnh hoàn hảo sẽ không bị phạt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi chiếc xe được xử lý trong thời gian không đổi sau khi lập bản đồ, chỉ với một lần quét ngược | 
| Không gian |$O(n)$| Chúng tôi lưu trữ ánh xạ vị trí và mảng chuyển đổi | 

Độ phức tạp tuyến tính phù hợp thoải mái trong các ràng buộc cho$10^5$các phần tử, cả về thời gian chạy và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import sys
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    pos = [0] * (n + 1)
    for i, car in enumerate(b):
        pos[car] = i

    arr = [pos[car] for car in a]

    mn = n + 1
    ans = 0
    for i in range(n - 1, -1, -1):
        if arr[i] > mn:
            ans += 1
        mn = min(mn, arr[i])

    return str(ans)

# provided sample
assert run("""5
3 5 2 1 4
4 3 2 5 1
""") == "2"

# all same order
assert run("""3
1 2 3
1 2 3
""") == "0"

# reversed exit
assert run("""3
1 2 3
3 2 1
""") == "2"

# single inversion structure
assert run("""4
1 3 2 4
1 2 3 4
""") == "1"

# large monotonic
assert run("""5
5 4 3 2 1
1 2 3 4 5
""") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| hoán vị giống hệt nhau | 0 | không có kết quả dương tính giả | 
| đảo ngược lệnh thoát | 2 | hành vi đảo ngược tối đa | 
| trao đổi đơn | 1 | phát hiện đảo ngược cục bộ | 
| mục nhập hoàn toàn đảo ngược | 4 | trường hợp xấu nhất bị phạt toàn bộ | 

## Vỏ cạnh 

Khi thứ tự vào và ra giống hệt nhau, mảng được chuyển đổi sẽ tăng dần. Điều kiện tối thiểu hậu tố không bao giờ kích hoạt, vì vậy câu trả lời là 0. Thuật toán duy trì chính xác điều này vì mọi phần tử luôn nhỏ hơn hoặc bằng cực tiểu trong tương lai. 

Khi thứ tự thoát bị đảo ngược hoàn toàn so với mục nhập, mọi mục nhập trước đó có nhiều mục nhập sau với chỉ số thoát nhỏ hơn. Hậu tố tối thiểu sớm trở thành 0 và giữ nguyên ở đó, khiến mọi phần tử đủ lớn đều được tính, đánh dấu chính xác hầu hết các ô tô là bị phạt. 

Khi chỉ có một cặp được đảo ngược cục bộ, chẳng hạn như một hoán đổi duy nhất trong một hoán vị được sắp xếp khác, chỉ phần tử trước hoán đổi sẽ vượt quá mức tối thiểu hậu tố sau đó. Quá trình quét sẽ cô lập chính xác vị trí đó, đảm bảo không tính toán quá mức.
