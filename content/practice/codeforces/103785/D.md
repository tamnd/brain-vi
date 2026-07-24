---
title: "CF 103785D - Anh Ninh"
description: "Chúng ta có một số khoảng nguyên đóng, mỗi khoảng được xác định bởi điểm cuối bên trái và điểm cuối bên phải. Nhiệm vụ là xác định có bao nhiêu số nguyên nằm trong mỗi khoảng này cùng một lúc. Nói cách khác, hãy tưởng tượng mỗi khoảng là một đoạn trên trục số."
date: "2026-07-02T08:52:08+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103785
codeforces_index: "D"
codeforces_contest_name: "CodeBrew : Freshers Contest 2022"
rating: 0
weight: 103785
solve_time_s: 46
verified: true
draft: false
---

[CF 103785D - Anh Cả Ning](https://codeforces.com/problemset/problem/103785/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một số khoảng nguyên đóng, mỗi khoảng được xác định bởi điểm cuối bên trái và điểm cuối bên phải. Nhiệm vụ là xác định có bao nhiêu số nguyên nằm trong mỗi khoảng này cùng một lúc. 

Nói cách khác, hãy tưởng tượng mỗi khoảng là một đoạn trên trục số. Chúng ta được yêu cầu tìm phần của đường thẳng chung cho tất cả các đoạn. Nếu có một phần chung như vậy thì chúng ta đếm xem có bao nhiêu số nguyên nằm trong đó. Nếu không có phần chung nào tồn tại thì câu trả lời là 0. 

Kích thước đầu vào đủ nhỏ để chúng tôi chỉ thực hiện một lần duy nhất trong tất cả các khoảng thời gian. Điều này ngay lập tức loại trừ bất cứ điều gì bậc hai hoặc liên quan đến việc so sánh từng cặp giữa các khoảng. Một giải pháp kiểm tra từng khoảng thời gian một lần là đủ. 

Một số trường hợp đặc biệt quan trọng. 

Nếu các khoảng hoàn toàn không trùng nhau, ví dụ [1, 2] và [5, 6], thì không có số nguyên nào thuộc cả hai và câu trả lời đúng là 0. Một sai lầm ngây thơ là tính toán thứ gì đó như tổng độ dài hợp hoặc quên rằng giao điểm trống phải được phát hiện rõ ràng. 

Nếu tất cả các khoảng chia sẻ ít nhất một điểm nhưng giao điểm co lại thành một số nguyên duy nhất, chẳng hạn như [3, 5], [4, 6], [5, 10], thì câu trả lời là 1. Điều này rất dễ bị xử lý sai nếu người ta tính sai sai khác mà không bao gồm cả hai điểm cuối. 

Một trường hợp khó phát hiện khác là khi điểm cuối rất lớn hoặc rất nhỏ. Logic chỉ phụ thuộc vào việc so sánh các điểm cuối, do đó phạm vi số nguyên không ảnh hưởng đến tính chính xác miễn là việc so sánh được xử lý đúng cách. 

## Phương pháp tiếp cận 

Một cách đơn giản để giải quyết vấn đề là nghĩ đến việc kiểm tra mọi số nguyên và xác minh xem nó có thuộc tất cả các khoảng hay không. Ý tưởng mạnh mẽ này sẽ yêu cầu xác định phạm vi tối thiểu tổng thể có thể có trên tất cả các khoảng và sau đó lặp qua từng số nguyên trong phạm vi đó để kiểm tra tư cách thành viên trong mỗi khoảng. 

Với mỗi số nguyên ứng cử viên x, chúng ta sẽ kiểm tra tất cả các khoảng và xác nhận xem L_i ≤ x ≤ R_i với mọi i hay không. Nếu có thì chúng tôi tính. 

Cách tiếp cận này đúng nhưng tốn kém. Nếu các điểm cuối của khoảng trải rộng trên một phạm vi lớn, chẳng hạn như lên tới 10^9, thì ngay cả một khoảng duy nhất cũng có thể buộc chúng ta phải xem xét tối đa 10^9 số nguyên ứng cử viên. Đối với mỗi ứng cử viên, việc kiểm tra tất cả các khoảng sẽ thêm một hệ số khác của m, khiến điều này hoàn toàn không khả thi. 

Quan sát quan trọng là tính thành viên trong tất cả các khoảng tương đương với việc thỏa mãn đồng thời tất cả các giới hạn dưới và tất cả các giới hạn trên. Một số x hợp lệ nếu nó lớn hơn hoặc bằng mọi điểm cuối bên trái và nhỏ hơn hoặc bằng mọi điểm cuối bên phải. Điều này chuyển đổi vấn đề từ việc kiểm tra mọi điểm sang tìm một phân đoạn bị ràng buộc duy nhất. 

Giới hạn bên phải nhỏ nhất có thể có của giao điểm là điểm tối thiểu của tất cả các điểm cuối bên phải, bởi vì bất kỳ điểm hợp lệ nào cũng phải nằm trong mỗi khoảng. Tương tự, giới hạn bên trái lớn nhất có thể có của giao lộ là điểm tối đa của tất cả các điểm cuối bên trái. Do đó, giao điểm là [max(L_i), min(R_i)]. Khi đã biết khoảng này, câu trả lời chỉ đơn giản là độ dài của nó nếu nó hợp lệ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(R−L) · m | O(1) | Quá chậm | 
| Tối ưu | O(m) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đọc tất cả các khoảng và khởi tạo hai biến: một để theo dõi điểm cuối bên trái tối đa và một để theo dõi điểm cuối bên phải tối thiểu. Chúng đại diện cho giới hạn giao lộ chặt chẽ nhất có thể được thấy cho đến nay. 
2. Đối với mỗi khoảng [l, r], cập nhật điểm cuối bên trái tối đa bằng cách so sánh nó với l. Điều này đảm bảo rằng ranh giới bên trái cuối cùng tôn trọng ràng buộc dưới mạnh nhất trong tất cả các khoảng. 
3. Đối với mỗi khoảng [l, r], cập nhật điểm cuối bên phải tối thiểu bằng cách so sánh nó với r. Điều này đảm bảo rằng ranh giới bên phải cuối cùng tôn trọng giới hạn trên mạnh nhất trong tất cả các khoảng. 
4. Sau khi xử lý tất cả các khoảng, diễn giải khoảng kết quả là [L, R], trong đó L là giá trị tối đa của tất cả các điểm cuối bên trái và R là giá trị tối thiểu của tất cả các điểm cuối bên phải. Khoảng này đại diện cho tất cả các số thỏa mãn mọi ràng buộc cùng một lúc. 
5. Nếu L lớn hơn R thì giao điểm trống nên xuất ra 0. 
6. Mặt khác, các số nguyên hợp lệ đều bao gồm các giá trị từ L đến R, do đó xuất ra R − L + 1. 

### Tại sao nó hoạt động 

Mỗi khoảng đóng góp hai ràng buộc độc lập: giới hạn dưới và giới hạn trên. Bất kỳ số nào thuộc tất cả các khoảng đều phải thỏa mãn mọi giới hạn dưới, giới hạn này trở thành ít nhất là giá trị tối đa của tất cả các điểm cuối bên trái và cũng phải thỏa mãn mọi giới hạn trên, giới hạn này trở thành tối đa là giá trị tối thiểu của tất cả các điểm cuối bên phải. Hai ràng buộc tổng hợp này mô tả chính xác giao điểm. Không có điều kiện ẩn nào khác tồn tại vì các khoảng chỉ áp đặt những hạn chế đơn điệu này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    m = int(input())
    
    L = -10**18
    R = 10**18
    
    for _ in range(m):
        l, r = map(int, input().split())
        L = max(L, l)
        R = min(R, r)
    
    if L > R:
        print(0)
    else:
        print(R - L + 1)

if __name__ == "__main__":
    solve()
```Việc triển khai trực tiếp phản ánh ý tưởng nén tất cả các giới hạn dưới thành một mức tối đa duy nhất và tất cả các giới hạn trên thành một mức tối thiểu duy nhất. Các giá trị ban đầu cho L và R được chọn đủ rộng để không ảnh hưởng đến bất kỳ phạm vi đầu vào hợp lệ nào. Mỗi lần cập nhật là thời gian không đổi, đảm bảo quét tuyến tính trong tất cả các khoảng thời gian. 

Phép trừ cuối cùng R − L + 1 đếm các số nguyên trong một khoảng đóng. +1 là cần thiết vì cả hai điểm cuối đều được bao gồm. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Khoảng đầu vào: 

[1, 5], [2, 6], [4, 10] 

Chúng tôi theo dõi L là điểm cuối bên trái tối đa và R là điểm tối thiểu của điểm cuối bên phải. 

| Bước | Khoảng thời gian | L (tối đa bên trái) | R (phút bên phải) | 
| --- | --- | --- | --- | 
| 1 | [1, 5] | 1 | 5 | 
| 2 | [2, 6] | 2 | 5 | 
| 3 | [4, 10] | 4 | 5 | 

Giao điểm cuối cùng là [4, 5] nên đáp án là 2. 

Điều này xác nhận rằng chỉ các giá trị đồng thời thỏa mãn tất cả các ràng buộc mới tồn tại và quá trình thu nhỏ nắm bắt chính xác sự chồng chéo. 

### Ví dụ 2 

Khoảng đầu vào: 

[1, 2], [5, 6], [3, 4] 

| Bước | Khoảng thời gian | L (tối đa bên trái) | R (phút bên phải) | 
| --- | --- | --- | --- | 
| 1 | [1, 2] | 1 | 2 | 
| 2 | [5, 6] | 5 | 2 | 
| 3 | [3, 4] | 5 | 2 | 

Kết quả cuối cùng có L = 5 và R = 2, do đó L > R và giao điểm trống. 

Thuật toán phát hiện chính xác rằng không có số nguyên nào có thể thỏa mãn tất cả các khoảng cùng một lúc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m) | Mỗi khoảng thời gian được xử lý một lần với các cập nhật liên tục | 
| Không gian | O(1) | Chỉ có hai biến theo dõi được sử dụng | 

Thuật toán này là tối ưu cho các ràng buộc đầu vào vì bất kỳ giải pháp nào ít nhất cũng phải đọc tất cả các khoảng thời gian vốn đã yêu cầu thời gian tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

def solve():
    m = int(input())
    L = -10**18
    R = 10**18
    for _ in range(m):
        l, r = map(int, input().split())
        L = max(L, l)
        R = min(R, r)
    if L > R:
        print(0)
    else:
        print(R - L + 1)

# sample-style tests
assert run("3\n1 5\n2 6\n4 10\n") == "2"
assert run("3\n1 2\n5 6\n3 4\n") == "0"

# custom tests
assert run("1\n10 20\n") == "11", "single interval"
assert run("2\n1 100\n50 60\n") == "11", "nested overlap"
assert run("3\n0 0\n0 0\n0 0\n") == "1", "all equal single point"
assert run("2\n-5 -1\n-3 2\n") == "3", "negative range overlap"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 khoảng | 11 | độ chính xác khoảng đơn | 
| chồng chéo lồng nhau | 11 | thu hẹp giao lộ thích hợp | 
| tất cả số không | 1 | nút giao một điểm | 
| phạm vi âm | 3 | xử lý các tiêu cực và giới hạn | 

## Vỏ cạnh 

Trường hợp một cạnh là khi chỉ có một khoảng. Thuật toán đặt L và R trực tiếp từ khoảng đó, do đó câu trả lời trở thành R − L + 1, đếm chính xác tất cả các số nguyên bên trong nó. 

Một trường hợp cạnh khác là khi các khoảng rời rạc sớm, chẳng hạn như [1, 2] theo sau là [100, 200]. Sau khi xử lý khoảng thứ hai, L trở thành 100 trong khi R vẫn là 2, do đó L > R ngay lập tức. Thuật toán xuất ra chính xác 0 mà không cần xử lý thêm. 

Trường hợp thứ ba là khi tất cả các khoảng thu gọn về một điểm chung duy nhất, chẳng hạn như [5, 5] được lặp lại nhiều lần. L và R vẫn bằng 5 và đầu ra trở thành 1, phản ánh chính xác một số nguyên hợp lệ.
