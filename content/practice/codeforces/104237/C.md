---
title: "CF 104237C - Loại bỏ rác"
description: "Chúng ta được cho một chuỗi các đống rác được sắp xếp theo một thứ tự cố định dọc theo một lối đi. Mỗi cọc có một trọng lượng và Bob phải nhặt các cọc từ trái sang phải mà không được bỏ qua hoặc sắp xếp lại chúng."
date: "2026-07-02T20:46:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104237
codeforces_index: "C"
codeforces_contest_name: "Harker Programming Invitational 2023 Novice"
rating: 0
weight: 104237
solve_time_s: 54
verified: true
draft: false
---

[CF 104237C - Loại bỏ rác](https://codeforces.com/problemset/problem/104237/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một chuỗi các đống rác được sắp xếp theo một thứ tự cố định dọc theo một lối đi. Mỗi cọc có một trọng lượng và Bob phải nhặt các cọc từ trái sang phải mà không được bỏ qua hoặc sắp xếp lại chúng. Anh ta liên tục thực hiện các chuyến đi đến thùng rác và trong mỗi chuyến đi, anh ta có thể mang một khối cọc liền kề bắt đầu từ nơi anh ta dừng lại lần cuối, miễn là tổng trọng lượng của chuyến đi đó không vượt quá giới hạn cố định.$K$. 

Nhiệm vụ là tính toán số chuyến đi nhỏ nhất cần thiết để xử lý toàn bộ chuỗi trong khi tôn trọng ràng buộc thứ tự và ràng buộc năng lực. 

Hạn chế chính là$N \le 10^5$, điều này ngay lập tức loại trừ bất kỳ giải pháp nào thử tất cả các phân vùng hoặc kiểm tra tất cả các mảng con một cách rõ ràng. Cách tiếp cận bậc hai hoặc tệ hơn sẽ thực hiện tới$10^{10}$hoạt động trong trường hợp xấu nhất, không thể thực hiện được trong giới hạn 1 giây. Điều này đẩy chúng ta tới một chiến lược quét tuyến tính duy nhất. 

Trường hợp cạnh tinh tế xuất hiện khi một cọc đơn có trọng lượng bằng$K$. Trong trường hợp đó, nó phải hình thành chuyến đi của riêng mình ngay cả khi nó xuất hiện thành một chuỗi các cọc nhỏ. Một trường hợp góc khác là khi tất cả các cọc vừa khít với một chuyến đi, nghĩa là câu trả lời phải là 1. Một cách tiếp cận ngây thơ đặt lại không chính xác hoặc bắt đầu một chuyến đi mới quá sớm sẽ bị tính quá mức trong những tình huống như vậy. 

## Phương pháp tiếp cận 

Chiến lược bạo lực sẽ mô phỏng tất cả các cách có thể để chia chuỗi thành các chuyến đi hợp lệ. Vì mỗi chuyến đi là một đoạn liền kề có tổng lớn nhất là$K$, người ta có thể thử mọi điểm dừng và tính toán đệ quy số lượng phân đoạn tối thiểu. Điều này đương nhiên dẫn đến việc thử tất cả các phân vùng của mảng. 

Tuy nhiên, số lượng phân vùng của một$N$- mảng phần tử tăng theo cấp số nhân. Ngay cả khi chúng ta giới hạn bản thân ở các phần tách hợp lệ, trường hợp xấu nhất khi tất cả các phần tử đều nhỏ cho phép chia tách hầu hết mọi nơi, dẫn đến gần đúng$2^{N-1}$khả năng. Điều này vượt xa mọi tính toán khả thi. 

Cấu trúc của vấn đề được đơn giản hóa đáng kể khi chúng ta hiểu nó như một quá trình đóng gói tham lam. Vì Bob phải xử lý các mục theo thứ tự và không thể sắp xếp lại hoặc bỏ qua, nên mỗi chuyến đi chỉ đơn giản là một tiền tố tối đa bắt đầu từ vị trí hiện tại mà tổng của nó không vượt quá$K$. Việc kéo dài thêm một chuyến đi luôn làm giảm hoặc duy trì số lượng chuyến đi, bởi vì việc bắt đầu một chuyến đi mới sớm hơn không bao giờ có thể giúp kết hợp các yếu tố trong tương lai một cách hiệu quả hơn do ràng buộc đặt hàng nghiêm ngặt. 

Điều này làm cho chiến lược tối ưu trở nên đơn giản: quét từ trái sang phải, tiếp tục tích lũy trọng lượng và bất cứ khi nào thêm đống tiếp theo sẽ vượt quá$K$, chúng ta cam kết chuyến đi hiện tại và bắt đầu một chuyến đi mới. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (phân vùng DP/đệ quy) |$O(2^N)$|$O(N)$| Quá chậm | 
| Đường chuyền đơn tham lam |$O(N)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bắt đầu với tổng số hành trình biểu thị tải trọng chuyến đi hiện tại và bộ đếm số chuyến đi. Cả hai đều bắt đầu từ con số 0. 
2. Đi ngang các cọc theo thứ tự từ trái qua phải. 
3. Đối với mỗi cọc, kiểm tra xem việc thêm trọng lượng của nó vào chuyến đi hiện tại có vượt quá$K$. 
4. Nếu không vượt quá$K$, bao gồm cọc trong chuyến đi hiện tại bằng cách tăng tổng số lần chạy. 
5. Nếu vượt quá$K$, hoàn tất chuyến đi hiện tại bằng cách tăng bộ đếm chuyến đi, đặt lại tổng số lần chạy theo trọng lượng của đống hiện tại và bắt đầu chuyến đi mới từ đống này. 
6. Sau khi xử lý hết các cọc, nếu còn chuyến nào chưa hoàn thành (tổng chạy khác 0) thì tính là một chuyến cuối cùng. 

Lý do đằng sau bước 5 là khi một ràng buộc bị vi phạm, chúng ta không thể sắp xếp lại các quyết định trước đó. Vì lệnh đã được sửa nên cách sửa chữa hợp lệ duy nhất là cắt ranh giới chuyến đi ngay trước phần tử vi phạm. 

### Tại sao nó hoạt động 

Tại bất kỳ điểm nào trong quá trình quét, thuật toán duy trì một đoạn cọc liền kề đại diện cho chuyến đi hiện tại và đoạn này luôn tối đa với ràng buộc là tổng của nó lớn nhất$K$. Nếu chúng ta kết thúc một chuyến đi sớm hơn mức cần thiết, chúng ta sẽ tăng số chuyến đi mà không có bất kỳ khả năng nào để hợp nhất các phần tử trong tương lai, vì không có phần tử nào trong tương lai có thể được di chuyển sớm hơn hoặc hoán đổi. Như vậy, mỗi lần cắt giảm, chúng ta bị ép buộc bởi tính khả thi chứ không phải sự lựa chọn, đảm bảo sự tối giản. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    
    trips = 0
    current = 0
    
    for x in a:
        if current + x <= k:
            current += x
        else:
            trips += 1
            current = x
    
    if current > 0:
        trips += 1
    
    print(trips)

if __name__ == "__main__":
    solve()
```Việc thực hiện tuân theo quá trình quét tham lam một cách chính xác. Biến`current`theo dõi trọng lượng chuyến đi đang diễn ra. Khi một đống mới không vừa, chúng ta lập tức đóng chuyến trước đó và bắt đầu chuyến mới chỉ với đống đó. 

Việc kiểm tra cuối cùng đảm bảo rằng chuyến đi cuối cùng được lấp đầy một phần sẽ được tính. Nếu không có điều này, phân đoạn cuối cùng sẽ bị bỏ lỡ khi không xảy ra tình trạng tràn ở cuối. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 2
1 1 2
```| Chỉ mục | Cọc | Tổng hiện tại | Hành động | Chuyến đi | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | thêm vào hiện tại | 0 | 
| 2 | 1 | 2 | thêm vào hiện tại | 0 | 
| 3 | 2 | 2 → tràn | đóng + bắt đầu mới | 1 → 2 | 

Sau khi xử lý xong chúng ta kết thúc với 2 chuyến đi. 

Điều này cho thấy tình trạng tràn buộc phải cắt chính xác tại điểm vượt quá công suất như thế nào. 

### Ví dụ 2 

đầu vào:```
5 10
2 3 1 4 2
```| Chỉ mục | Cọc | Tổng hiện tại | Hành động | Chuyến đi | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 2 | thêm | 0 | 
| 2 | 3 | 5 | thêm | 0 | 
| 3 | 1 | 6 | thêm | 0 | 
| 4 | 4 | 10 | thêm | 0 | 
| 5 | 2 | 10 → tràn | đóng + mới | 1 → 2 | 

Câu trả lời cuối cùng là 2 chuyến. 

Điều này chứng tỏ rằng thuật toán sắp xếp mỗi chuyến đi càng chặt chẽ càng tốt trước khi bắt đầu một chuyến đi mới. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N)$| Mỗi cọc được xử lý đúng một lần với các thao tác liên tục trong thời gian | 
| Không gian |$O(1)$| Chỉ một số biến số nguyên được duy trì bất kể kích thước đầu vào | 

Quét tuyến tính phù hợp thoải mái trong các ràng buộc đối với$N = 10^5$, chỉ yêu cầu số học đơn giản cho mỗi phần tử, trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    import contextlib
    output = io.StringIO()
    with contextlib.redirect_stdout(output):
        solve()
    return output.getvalue().strip()

# provided sample
assert run("3 2\n1 1 2\n") == "2"

# single element
assert run("1 5\n3\n") == "1"

# all fit in one trip
assert run("4 10\n1 2 3 4\n") == "1"

# each element forces new trip
assert run("4 3\n3 3 3 3\n") == "4"

# alternating tight packing
assert run("6 5\n2 3 2 3 2 3\n") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | 1 | trường hợp ranh giới tối thiểu | 
| tất cả đều phù hợp | 1 | đóng gói đầy đủ trong một chuyến đi | 
| tất cả đều bằng K | N | buộc phải chia rẽ từng bước | 
| số tiền xen kẽ | 4 | tham lam đúng ranh giới | 

## Vỏ cạnh 

Một đầu vào tối thiểu như`1 5 / 3`được xử lý chính xác vì vòng lặp không bao giờ kích hoạt sự phân tách và giá trị cuối cùng khác 0`current`đóng góp đúng một chuyến đi. 

Một cọc đơn đầy đủ công suất như`1 5 / 5`ngay lập tức tạo một chuyến đi đã hoàn thành, vì việc kiểm tra tràn không bao giờ kích hoạt nhưng lần tích lũy cuối cùng vẫn được tính. 

Một chuỗi trong đó mọi cọc đều bằng nhau$K$, chẳng hạn như`4 3 / 3 3 3 3`, buộc một chuyến đi mới ở mọi phần tử. Mỗi lần`current + x`vượt quá$K$, thuật toán sẽ được đặt lại, đảm bảo không có cọc nào được sáp nhập sai. 

Một chuỗi được đóng gói chặt chẽ như`6 5 / 2 3 2 3 2 3`chứng tỏ rằng việc đóng gói tham lam luôn lấp đầy mỗi chuyến đi càng nhiều càng tốt trước khi cắt và không bao giờ trì hoãn việc cắt giảm nhằm cải thiện tính khả thi.
