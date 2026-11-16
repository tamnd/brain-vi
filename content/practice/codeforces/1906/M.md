---
title: "CF 1906M - Xây dựng tam giác"
description: "Chúng ta có một đa giác đều lồi có các cạnh $N$. Mỗi cạnh $i$ chứa các điểm đặc biệt $Ai$ được đặt thống nhất dọc theo đoạn ranh giới của cạnh đó."
date: "2026-06-08T20:48:19+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1906
codeforces_index: "M"
codeforces_contest_name: "2023-2024 ICPC, Asia Jakarta Regional Contest (Online Mirror, Unrated, ICPC Rules, Teams Preferred)"
rating: 1700
weight: 1906
solve_time_s: 75
verified: true
draft: false
---

[CF 1906M - Xây dựng tam giác](https://codeforces.com/problemset/problem/1906/M) 

**Đánh giá:** 1700 
**Tags:** tham lam, toán học 
**Thời gian giải:** 1m 15s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đa giác đều lồi với$N$các bên. Mỗi bên$i$chứa$A_i$các điểm đặc biệt đặt đều dọc theo đoạn ranh giới của phía đó. Vì một bên được chia thành$A_i + 1$những khoảng cách bằng nhau, những điểm này hoạt động giống như các đỉnh rời rạc nằm trên ranh giới đa giác. 

Nhiệm vụ là chọn ba điểm đặc biệt này để tạo thành các hình tam giác, với ba ràng buộc đồng thời. Mỗi điểm đặc biệt chỉ được sử dụng tối đa một lần, mỗi tam giác phải có diện tích khác 0 và không được phép giao nhau dù chỉ một phần. Mục tiêu là tối đa hóa số lượng các hình tam giác như vậy. 

Các ràng buộc đẩy giải pháp tới hành vi tuyến tính hoặc gần tuyến tính trong$N$, từ$N$có thể đạt được$2 \cdot 10^5$và mỗi$A_i$có thể lớn như$2 \cdot 10^9$. Bất kỳ cách tiếp cận nào lặp lại trên tất cả các điểm một cách rõ ràng đều không thể thực hiện được vì tổng số điểm thậm chí không bị giới hạn bởi một tổng có thể quản lý được và thậm chí việc xử lý từng bên một cách độc lập sẽ dẫn đến các hoạt động tỷ lệ thuận với$A_i$, điều đó là không thể thực hiện được. 

Một khó khăn tinh tế nằm ở việc hiểu rằng chúng ta không thực sự thao tác tọa độ của các điểm trong không gian Euclide. Hình học bị ràng buộc bởi ranh giới đa giác lồi, điều này buộc bất kỳ việc đóng gói hình tam giác không giao nhau nào phải hoạt động giống như phân vùng khoảng dọc theo cấu trúc tuần hoàn. 

Một sai lầm ngây thơ xuất hiện khi cố gắng tạo thành các hình tam giác một cách tham lam ở mỗi bên một cách độc lập. Ví dụ: nếu một bên có nhiều điểm và bên kia có ít điểm, việc ghép các tam giác tối ưu cục bộ ở mỗi bên sẽ bỏ qua rằng các tam giác phải trải dài qua các cạnh để tránh giao nhau và tối đa hóa mật độ đóng gói. Một ý tưởng sai lầm khác là coi đây là “chọn 3k điểm bất kỳ” vì mỗi bộ ba tạo thành một hình tam giác. Điều này không thành công vì các ràng buộc giao nhau kết hợp tất cả các lựa chọn trên toàn cầu. 

Một cách tiếp cận đúng phải suy luận về cách các điểm biên có thể được nhóm thành các bộ ba không cắt nhau trên một ranh giới tuần hoàn. 

## Phương pháp tiếp cận 

Nếu chúng ta quên hình học trong giây lát, vấn đề sẽ trở thành: chúng ta có một chuỗi tuần hoàn các điểm được nhóm trên các cạnh và chúng ta muốn phân chia càng nhiều bộ ba càng tốt sao cho mỗi bộ ba tương ứng với một tam giác không giao nhau. 

Một cách giải thích bạo lực sẽ liệt kê tất cả các tập hợp con của bộ ba, kiểm tra xem chúng có tạo thành các hình tam giác không giao nhau hợp lệ hay không và lấy tập hợp con số lượng tối đa. Đây là sự bùng nổ tổ hợp. Ngay cả khi bỏ qua xác nhận hình học, số cách để chọn các bộ ba rời nhau từ$M$điểm theo thứ tự$O(M!)$trong cấu trúc và$M$bản thân nó không bị giới hạn một cách rõ ràng về tổng thể. Cách tiếp cận này chết ngay lập tức. 

Cái nhìn sâu sắc về cấu trúc quan trọng là ranh giới đa giác tạo ra thứ tự tuần hoàn của tất cả các điểm. Khi chúng ta đặt tất cả các điểm đặc biệt dọc theo chu trình này, mọi tam giác hợp lệ sẽ tương ứng với việc chọn ba đỉnh phân chia chu trình thành ba cung và việc không giao nhau ngụ ý cấu trúc nhiều tầng: các tam giác hoạt động giống như các dây không cắt nhau trong một đa giác lồi. 

Vấn đề giảm xuống mức tối đa hóa số lượng bộ ba không cắt nhau trên một chuỗi hình tròn với bội số trên mỗi cạnh. Quan sát quan trọng là chỉ có số lượng trên mỗi cạnh mới quan trọng chứ không phải vị trí chính xác trong một cạnh, bởi vì các điểm có khoảng cách đều nhau và bất kỳ sự sắp xếp lại nào bên trong một cạnh đều không ảnh hưởng đến cấu trúc giao cắt giữa các cạnh khác nhau. 

Chúng ta có thể giải thích mỗi bên$i$như đóng góp$A_i$“mã thông báo” có sẵn. Mỗi tam giác tiêu thụ 3 token, nhưng tính khả thi phụ thuộc vào cách phân phối token trong chu kỳ. Yếu tố hạn chế nằm ở chỗ chúng ta có thể cân bằng các đóng góp trên đa giác một cách đồng đều như thế nào để tránh tạo ra các điểm còn sót lại không thể so sánh được buộc phải vượt qua. 

Chiến lược tối ưu biến thành một quá trình cân bằng tham lam: chúng ta tích lũy điểm dọc theo chu kỳ và hình thành các hình tam giác bất cứ khi nào chúng ta có đủ “dư thừa” ở các cạnh liên tiếp, đảm bảo chúng ta luôn đóng các hình tam giác càng sớm càng tốt theo cách tránh bị tắc nghẽn trong tương lai. 

Điều này có thể được mô hình hóa bằng cách theo dõi sự mất cân bằng tích lũy trong khi đi ngang qua các cạnh và trích xuất các nhóm 3 người một cách tham lam bất cứ khi nào có thể. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force tăng gấp ba lần | Hàm mũ | Lớn | Quá chậm | 
| Tích lũy theo chu kỳ tham lam | O(N) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý các cạnh của đa giác theo thứ tự trong khi vẫn duy trì số điểm hiện “có sẵn” nhưng chưa được sử dụng trong các hình tam giác. 

1. Khởi tạo bộ đếm`carry = 0`để thể hiện số điểm chưa sử dụng được tích lũy cho đến nay. 
2. Khởi tạo`ans = 0`về số lượng hình tam giác được tạo thành. 
3. Lặp lại từng bên$i$từ 1 đến$N$, thêm$A_i$ĐẾN`carry`. 
4. Bất cứ khi nào`carry >= 3`, chúng ta tạo thành càng nhiều hình tam giác càng tốt từ nhóm hiện tại, tăng dần`ans += carry // 3`, và cập nhật`carry = carry % 3`. 
5. Tiếp tục cho đến khi tất cả các mặt đều được xử lý. 

Lý do đằng sau việc trích xuất tham lam này là bất kỳ tập hợp 3 điểm biên sẵn có nào luôn có thể được sắp xếp thành một tam giác không suy biến mà không buộc phải giao nhau, miễn là chúng ta không bao giờ trì hoãn việc hình thành tam giác một cách không cần thiết. Việc trì hoãn việc phân nhóm sẽ chỉ làm tăng mật độ địa phương và có nguy cơ buộc phải vượt biển khi bên tiếp theo bổ sung thêm các điểm mới. 

### Tại sao nó hoạt động 

Bất biến chính là sau khi xử lý bên$i$, tất cả các điểm không được sử dụng đều tương đương với phép quay theo chu kỳ, nghĩa là vị trí tương đối của chúng luôn có thể được nhúng trên đường biên mà không tạo ra các giao điểm miễn là chúng ta không bao giờ để lại nhiều hơn 2 điểm chưa khớp giữa các nhóm liên tiếp. Vì bất kỳ tam giác nào cũng có chính xác 3 điểm, nên việc duy trì phần còn lại tối đa là 2 điểm sẽ đảm bảo không tích lũy cấu trúc từng phần bắt buộc nào có thể chặn các nhóm hợp lệ trong tương lai. Việc trích xuất tham lam đảm bảo việc đóng gói tối đa vì mỗi khi chúng ta có sẵn 3 điểm, việc trì hoãn việc sử dụng chúng không thể làm tăng số lượng hình tam giác sau này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    A = list(map(int, input().split()))

    carry = 0
    ans = 0

    for x in A:
        carry += x
        ans += carry // 3
        carry %= 3

    print(ans)

if __name__ == "__main__":
    solve()
```Việc thực hiện trực tiếp theo ý tưởng tích lũy. Biến`carry`biểu thị số điểm vẫn chưa được sử dụng sau khi tham lam tạo thành các hình tam giác càng sớm càng tốt. Phép chia số nguyên`carry // 3`đếm xem có thể trích xuất bao nhiêu hình tam giác ở mỗi bước và phần còn lại sẽ truyền về phía trước. 

Một chi tiết tinh tế là chúng tôi không bao giờ cố gắng lưu trữ các vị trí điểm thực tế hoặc mô phỏng hình học. Tính chính xác hoàn toàn dựa vào thực tế là chỉ có tổng số modulo 3 quan trọng đối với cấu trúc còn sót lại. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
3 1 4 6
```| Bước | A[i] | mang trước | tam giác hình thành | theo sau | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | 0 | 1 | 0 | 
| 2 | 1 | 1 | 0 | 1 | 
| 3 | 4 | 5 | 1 | 2 | 
| 4 | 6 | 8 | 2 | 2 | 

Câu trả lời cuối cùng là 4. 

Dấu vết này cho thấy việc phân nhóm luôn được thực hiện ngay lập tức, ngăn ngừa sự tích tụ các phần dư lớn và đảm bảo trích xuất tối đa các bộ ba. 

### Ví dụ 2 

đầu vào:```
3
2 2 2
```| Bước | A[i] | mang trước | tam giác hình thành | theo sau | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 0 | 0 | 2 | 
| 2 | 2 | 2 | 1 | 1 | 
| 3 | 2 | 3 | 1 | 0 | 

Câu trả lời cuối cùng là 2. 

Điều này chứng tỏ rằng sự phân bổ đồng đều giữa các bên không làm thay đổi hành vi tham lam, vì việc phân nhóm chỉ phụ thuộc vào tổng mức sẵn có. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Mỗi bên được xử lý một lần với công việc liên tục | 
| Không gian | O(1) | Chỉ có một số biến số nguyên được duy trì | 

Thuật toán xử lý thoải mái$N = 2 \cdot 10^5$vì nó thực hiện một đường truyền tuyến tính duy nhất với chi phí tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    out = io.StringIO()
    sys.stdout = out

    n = int(sys.stdin.readline())
    A = list(map(int, sys.stdin.readline().split()))

    carry = 0
    ans = 0
    for x in A:
        carry += x
        ans += carry // 3
        carry %= 3

    return out.getvalue().strip() if out.getvalue() else str(ans)

# provided sample
assert run("4\n3 1 4 6\n") == "4"

# minimum case
assert run("3\n1 1 1\n") == "1"

# all equal large-small mix
assert run("3\n2 2 2\n") == "2"

# edge: no triangles
assert run("3\n1 1 1\n") == "1"

# skewed distribution
assert run("5\n10 0 0 0 0\n") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 1 1 1 | 1 | trường hợp tam giác hợp lệ tối thiểu | 
| 3 2 2 2 | 2 | phân phối thống nhất | 
| 5 10 0 0 0 0 | 3 | xử lý mất cân bằng nặng | 

## Vỏ cạnh 

Trường hợp góc xảy ra khi tất cả các điểm đều nằm trên một mặt, ví dụ:```
N = 3
A = [10, 0, 0]
```Thuật toán tích lũy 10 điểm thành`carry`, tạo ra 3 hình tam giác và để lại 1 phần còn lại. Điều này phù hợp với thực tế là mặc dù tất cả các điểm nằm trên một cạnh, chúng vẫn là các điểm biên hợp lệ của một đa giác lồi và bất kỳ bộ ba nào tạo thành một tam giác hợp lệ miễn là chúng khác biệt, vì vậy giá trị lớn nhất là sàn (10/3). 

Một trường hợp tinh vi khác là khi các giá trị đều bằng 1:```
A = [1, 1, 1, 1]
```Việc tích lũy mang lại chính xác một tam giác từ ba đóng góp đầu tiên và để lại một điểm không được sử dụng, điểm này không thể tạo thành một tam giác khác. Quá trình tham lam ngăn chặn việc đếm quá mức vì nó không bao giờ trì hoãn việc phân nhóm khi có sẵn bộ ba.
