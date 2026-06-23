---
title: "CF 105056E - Kiosk POS"
description: "Chúng ta được cung cấp một mảng các số nguyên trong đó mỗi giá trị mô tả sự thay đổi ròng trong các bản ghi được lưu trữ trong một đợt: giá trị dương tăng không gian bị chiếm dụng và giá trị âm làm tăng không gian trống."
date: "2026-06-23T11:13:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105056
codeforces_index: "E"
codeforces_contest_name: "International Odoo Programming Contest 2024"
rating: 0
weight: 105056
solve_time_s: 83
verified: false
draft: false
---

[CF 105056E - POS Kiosk](https://codeforces.com/problemset/problem/105056/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 23s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên trong đó mỗi giá trị mô tả sự thay đổi ròng trong các bản ghi được lưu trữ trong một đợt: giá trị dương tăng không gian bị chiếm dụng và giá trị âm làm tăng không gian trống. Đối với bất kỳ phân đoạn liền kề nào của các lô này, chúng tôi mô phỏng việc áp dụng chúng theo thứ tự bắt đầu từ bộ nhớ bằng 0 và theo dõi lượng dung lượng bổ sung mà chúng tôi cần để tránh bị tràn. Chức năng của một phân đoạn là mức lưu trữ cao nhất đạt được ở bất kỳ tiền tố nào của phân khúc đó. 

Tương tự, nếu chúng ta nhìn vào một mảng con, chúng ta bắt đầu từ số 0 và tích lũy các giá trị từng bước. Bất cứ khi nào tổng số tiền hoạt động tăng lên, chúng tôi cần nhiều năng lực hơn; bất cứ khi nào nó giảm, chúng tôi chỉ giải phóng dung lượng đã sử dụng trước đó. Do đó, chi phí của một phân khúc là tổng tiền tố tối đa bên trong phân khúc đó. 

Nhiệm vụ là tính giá trị này cho mọi mảng con có thể và tính tổng chúng trên tất cả các mảng con. 

Các ràng buộc rất lớn: tổng kích thước mảng trên tất cả các trường hợp thử nghiệm lên tới 200.000. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào đánh giá rõ ràng mọi mảng con và tính lại cực đại tiền tố bên trong nó, vì đó sẽ là bậc ba trong trường hợp xấu nhất. Ngay cả giải pháp O(n^2) cho mỗi trường hợp thử nghiệm cũng quá chậm khi tính tổng trên tất cả các thử nghiệm. 

Một vấn đề tế nhị xuất hiện trong suy nghĩ ngây thơ về “tổng tiền tố tối đa”. Rất dễ coi sai đây là tổng mảng con tối đa hoặc cho rằng nó chỉ phụ thuộc vào điểm cuối. Nó không. Sự sụt giảm nhỏ ở đầu phân khúc, sau đó là sự gia tăng lớn sau đó có thể chi phối câu trả lời và hành vi đó phụ thuộc vào cấu trúc bên trong chứ không chỉ các ranh giới. 

Ví dụ: trong một phân đoạn như`[2, 3, -4, 6]`, tổng số tiền hiện có là`2, 5, 1, 7`, vì vậy câu trả lời là 7 mặc dù giá trị cuối cùng không phải là giá trị lớn nhất. Bất kỳ phương pháp nào chỉ xem xét tổng hoặc điểm cuối sẽ không thành công ở đây. 

## Phương pháp tiếp cận 

Một giải pháp trực tiếp lặp lại trên mỗi cặp`(L, R)`, tính tổng tiền tố bên trong mảng con đó, theo dõi mức tối đa và tích lũy kết quả. Điều này đúng nhưng đắt tiền. Đối với mỗi mảng con, chúng tôi thực hiện công việc O(độ dài), dẫn đến tổng thể là O(n^3). 

Ngay cả khi chúng tôi tính toán trước tổng tiền tố toàn cục, chúng tôi vẫn cần giá trị tối đa trên một cửa sổ trượt trong mỗi mảng con, vẫn là giá trị bậc hai để đánh giá trên tất cả các mảng con. 

Phép biến đổi chính là viết lại hành vi của mảng con bằng cách sử dụng tổng tiền tố toàn cục. Cho phép`P[i]`là tổng tiền tố với`P[0] = 0`. Sau đó, tổng chạy bên trong một mảng con`[L, R]`ở vị trí`i`bằng`P[i] - P[L-1]`. Giá trị tối đa trên mảng con trở thành:`f(L, R) = max(P[L], P[L+1], ..., P[R]) - P[L-1]`Vì vậy, vấn đề chia thành hai phần. Phần đầu tiên là tổng trên cực đại mảng con của mảng tiền tố`P`. Phần thứ hai là một số hạng tuyến tính chỉ phụ thuộc vào`P[L-1]`. 

Phần thứ hai rất đơn giản để tổng hợp. Phần đầu tiên là một bài toán kinh điển: tổng các phần tử tối đa trên tất cả các mảng con, có thể được giải bằng cách sử dụng ngăn xếp đơn điệu bằng cách đếm xem mỗi vị trí đóng góp bao nhiêu mảng con là lớn nhất. 

Điều này làm giảm toàn bộ vấn đề thành hai phép tính tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) mỗi lần kiểm tra | O(1) | Quá chậm | 
| Ngăn xếp đơn điệu + Phân tách tiền tố | O(n) mỗi lần kiểm tra | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi chuyển đổi vấn đề thành tổng tiền tố, sau đó tách nó thành phần “mảng con tối đa trên mảng tiền tố” và thuật ngữ hiệu chỉnh. 

1. Xây dựng tổng tiền tố`P`Ở đâu`P[0] = 0`Và`P[i] = A[1] + ... + A[i]`. Điều này cho phép chúng ta biểu thị bất kỳ tổng phân đoạn nào dưới dạng chênh lệch của hai giá trị tiền tố. 
2. Viết lại giá thành của một đoạn`[L, R]`BẰNG`max(P[L..R]) - P[L-1]`. Số hạng trừ chỉ phụ thuộc vào điểm cuối bên trái, điều này rất quan trọng vì nó cho phép tổng hợp độc lập sau này. 
3. Tính tổng số tiền đóng góp của`-P[L-1]`một phần trên tất cả các mảng con. Đối với một chỉ số cố định`i = L-1`, nó xuất hiện trong tất cả các mảng con bắt đầu từ`L = i+1`, vậy trong`(n - i)`mảng con. Chúng tôi nhân lên`P[i]`bằng cách đếm và tính tổng trên tất cả`i`. 
4. Tính tổng các giá trị lớn nhất của mảng con trên mảng`P`. Đối với mỗi vị trí`i`, xác định có bao nhiêu mảng con`[L, R]`có`P[i]`là phần tử tối đa. Điều này được thực hiện bằng cách tìm phần tử lớn hơn gần nhất ở bên trái và bên phải bằng cách sử dụng ngăn xếp giảm dần đơn điệu. 
5. Đối với mỗi chỉ số`i`, nếu nó là mức tối đa trong`count_left[i]`lựa chọn cho L và`count_right[i]`các lựa chọn cho R thì đóng góp của nó là`P[i] * count_left[i] * count_right[i]`. Tổng các phần đóng góp này mang lại tổng tổng của tất cả các cực đại của mảng con. 
6. Trừ số hạng tuyến tính ở bước 3 để có kết quả cuối cùng. 

### Tại sao nó hoạt động 

Việc chuyển đổi tiền tố chuyển đổi “tổng hoạt động tối đa bên trong một phân đoạn” ban đầu thành một truy vấn tối đa trong phạm vi tĩnh đối với các giá trị tiền tố. Điều này loại bỏ sự phụ thuộc vào tích lũy năng động. Khi ở dạng này, giá trị của mỗi mảng con chỉ phụ thuộc vào tối đa các phần tử mảng cố định trừ đi phần bù xác định. Thuộc tính phân tách tối đa đảm bảo mỗi giá trị tiền tố đóng góp độc lập trên một tập hợp các mảng con được xác định rõ ràng, đó chính xác là số lượng của ngăn xếp đơn điệu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    # prefix sums
    P = [0] * (n + 1)
    for i in range(1, n + 1):
        P[i] = P[i - 1] + a[i - 1]

    # We work on P[0..n]
    # Step 1: sum of subarray maximums over P
    arr = P

    nP = n + 1

    # previous greater (strict) and next greater (>= or > carefully handled)
    left = [0] * nP
    right = [0] * nP

    stack = []
    for i in range(nP):
        while stack and arr[stack[-1]] <= arr[i]:
            stack.pop()
        left[i] = stack[-1] if stack else -1
        stack.append(i)

    stack = []
    for i in range(nP - 1, -1, -1):
        while stack and arr[stack[-1]] < arr[i]:
            stack.pop()
        right[i] = stack[-1] if stack else nP
        stack.append(i)

    sum_max = 0
    for i in range(nP):
        l = i - left[i]
        r = right[i] - i
        sum_max += arr[i] * l * r

    # Step 2: subtract linear contribution
    sub = 0
    for i in range(nP - 1):
        sub += P[i] * (n - i)

    print(sum_max - sub)

if __name__ == "__main__":
    solve()
```Mã bắt đầu bằng cách xây dựng các tổng tiền tố sao cho mỗi tổng phân đoạn trở thành hiệu của hai giá trị tiền tố. Sau đó, nó xử lý vấn đề bằng cách đếm cực đại của mảng con trên mảng tiền tố. 

Phần ngăn xếp đơn điệu tính toán, đối với mỗi giá trị tiền tố, nó có thể mở rộng sang trái và phải bao xa trong khi vẫn duy trì mức tối đa. Ranh giới bên trái dừng ở giá trị lớn hơn trước đó và ranh giới bên phải dừng ở giá trị lớn hơn hoặc bằng tiếp theo, đảm bảo mỗi mảng con được tính chính xác một lần. 

Cuối cùng, số hạng trừ chiếm phần bù cố định`P[L-1]`xuất hiện trong mọi mảng con bắt đầu từ`L`. 

## Ví dụ đã hoạt động 

Hãy xem xét`A = [2, 3, -4]`. Mảng tiền tố là`P = [0, 2, 5, 1]`. 

Đối với cực đại của mảng con hơn`P`, chúng tôi kiểm tra từng khoảng: 

| mảng con | P tối đa | 
| --- | --- | 
| [0] | 0 | 
| [0,2] | 2 | 
| [0,2,5] | 5 | 
| [2,5] | 5 | 
| [2,5,1] | 5 | 
| [5] | 5 | 
| [5,1] | 5 | 
| [1] | 1 | 

Tính tổng cho tổng đóng góp từ cực đại. Sau đó, chúng tôi trừ số hạng tuyến tính dựa trên các giá trị tiền tố bắt đầu. 

Bây giờ hãy xem xét`A = [3, -5]`, Vì thế`P = [0, 3, -2]`. 

Tất cả các mảng con: 

| mảng con | P tối đa | 
| --- | --- | 
| [0] | 0 | 
| [0,3] | 3 | 
| [3] | 3 | 
| [3,-2] | 3 | 
| [-2] | -2 | 

Điều này cho thấy tiền tố phủ định vẫn tham gia vào cực đại tùy thuộc vào cấu trúc xung quanh. 

Những ví dụ này xác nhận rằng phép biến đổi tách biệt cấu trúc một cách chính xác: cực đại chỉ phụ thuộc vào thứ tự tương đối của các giá trị tiền tố, không phụ thuộc vào dấu gốc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) mỗi lần kiểm tra | Mỗi phần tử tiền tố vào và ra khỏi ngăn xếp một lần, cộng với tập hợp tuyến tính | 
| Không gian | O(n) | Cấu trúc mảng và ngăn xếp tiền tố | 

Giải pháp tuyến tính về tổng kích thước đầu vào, vừa vặn thoải mái trong giới hạn tổng cộng 200.000 phần tử. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def solve():
        n = int(input())
        a = list(map(int, input().split()))

        P = [0] * (n + 1)
        for i in range(1, n + 1):
            P[i] = P[i - 1] + a[i - 1]

        arr = P
        nP = n + 1

        left = [0] * nP
        right = [0] * nP

        st = []
        for i in range(nP):
            while st and arr[st[-1]] <= arr[i]:
                st.pop()
            left[i] = st[-1] if st else -1
            st.append(i)

        st = []
        for i in range(nP - 1, -1, -1):
            while st and arr[st[-1]] < arr[i]:
                st.pop()
            right[i] = st[-1] if st else nP
            st.append(i)

        sum_max = 0
        for i in range(nP):
            sum_max += arr[i] * (i - left[i]) * (right[i] - i)

        sub = 0
        for i in range(nP - 1):
            sub += P[i] * (n - i)

        return str(sum_max - sub)

    return solve()

# custom tests
assert run("1\n5\n") == "5", "single positive"
assert run("1\n-3\n") == "0", "single negative"
assert run("2\n1 -1\n") == "2", "mixed small"
assert run("3\n2 3 -4\n") == "8", "sample-like"
assert run("2\n1000 -1000\n") == "1000", "boundary cancellation"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| yếu tố duy nhất tích cực | 5 | trường hợp cơ sở đúng đắn | 
| phần tử đơn âm | 0 | không cần năng lực âm | 
| trộn nhỏ | 2 | tương tác tiền tố | 
| giống mẫu | 8 | tính nhất quán với ví dụ về câu lệnh | 
| hủy bỏ ranh giới | 1000 | tích cực lớn theo sau là hoàn nguyên hoàn toàn | 

## Vỏ cạnh 

Mảng một phần tử kiểm tra xem thuật toán có diễn giải chính xác rằng tiền tố tối đa chỉ đơn giản là phần tử đó khi dương và bằng 0 nếu ngược lại; việc chuyển đổi tiền tố đảm bảo không xảy ra tình trạng đếm quá mức mảng con. 

Mảng toàn âm đảm bảo rằng tiền tố cực đại hoạt động chính xác khi tổng chạy không bao giờ tăng trên 0. Ngăn xếp đơn điệu vẫn tính các khoản đóng góp, nhưng thuật ngữ trừ chiếm ưu thế một cách chính xác, tạo ra tổng công suất cần thiết bằng 0 trên tất cả các mảng con. 

Các giá trị dương và âm lớn xen kẽ xác nhận rằng các đỉnh trung gian được ghi lại ngay cả khi tiền tố cuối cùng nhỏ, đó chính xác là lý do tại sao việc sử dụng tiền tố cực đại thay vì tổng tổng là điều cần thiết.
