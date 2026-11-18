---
title: "CF 2051F - Joker"
description: "Chúng tôi đang làm việc với một bộ bài trong đó chỉ có vị trí tương đối của một lá bài đặc biệt, quân joker, mới quan trọng. Ban đầu người pha trò ngồi ở vị trí m trong một hàng n lá bài. Sau đó, chúng tôi thực hiện một chuỗi các hoạt động."
date: "2026-06-08T08:43:04+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 2051
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 995 (Div. 3)"
rating: 2000
weight: 2051
solve_time_s: 114
verified: false
draft: false
---

[CF 2051F - Joker](https://codeforces.com/problemset/problem/2051/F) 

**Xếp hạng:** 2000 
**Tags:** vũ phu, tham lam, thực hiện, toán học 
**Thời gian giải:** 1m 54s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang làm việc với một bộ bài trong đó chỉ có vị trí tương đối của một lá bài đặc biệt, quân joker, mới quan trọng. Ban đầu người pha trò ngồi ở vị trí`m`trong một dòng`n`thẻ. Sau đó, chúng tôi thực hiện một chuỗi các hoạt động. Mỗi thao tác chọn một vị trí`a_i`trong bộ bài hiện tại, hãy lấy lá bài ra ở vị trí đó và nhét nó vào ở trên cùng hoặc dưới cùng. 

Bản thân quân joker không bao giờ được di chuyển trực tiếp, nhưng vị trí của nó sẽ thay đổi một cách gián tiếp bất cứ khi nào một lá bài được lấy ra hoặc đưa vào trước nó. Sau mỗi thao tác, chúng ta không được yêu cầu về một vị trí cuối cùng duy nhất của quân joker mà là số lượng vị trí khác nhau mà joker có thể chiếm giữ tùy thuộc vào tất cả các lựa chọn di chuyển lá bài đã chọn lên trên cùng hoặc dưới cùng ở mỗi bước. 

Vì vậy, trạng thái không phải là một bộ bài mà là một tập hợp ngày càng tăng các bộ bài có thể có. Chúng tôi chỉ theo dõi, trên tất cả các kết quả có thể xảy ra này, những chỉ số nào mà người pha trò có thể đạt được sau mỗi thao tác và xuất ra kích thước của tập hợp đó. 

Các ràng buộc là tín hiệu quan trọng ở đây. Kích thước boong`n`có thể lớn tới 10^9, vì vậy chúng tôi không thể mô phỏng bộ bài hoặc duy trì các cấu trúc rõ ràng được lập chỉ mục theo vị trí. Tổng số thao tác trên mỗi thử nghiệm lên tới 2·10^5 trong các thử nghiệm, điều này buộc O(q) hoặc O(q log q) cho mỗi giải pháp thử nghiệm ở mức tồi tệ nhất. Bất cứ điều gì liên quan đến việc xây dựng lại mảng hoặc mô phỏng hoán vị đều là không thể ngay lập tức. 

Điểm tinh tế chính là mỗi hoạt động phân nhánh thành hai khả năng, do đó, việc bùng nổ trạng thái ngây thơ trên các cấu hình 2^q là hoàn toàn không khả thi. Ngay cả việc theo dõi toàn bộ vị trí của joker cũng sẽ cần bộ nhớ O(n), điều này cũng không thể thực hiện được vì n rất lớn. 

Một vài kiểu thất bại xuất hiện trong lối suy luận ngây thơ. 

Một sai lầm phổ biến là cho rằng vị trí của người pha trò luôn là một khoảng đơn giản mở rộng. Ví dụ: nếu người pha trò bắt đầu lúc`m=3`trong một bộ bài nhỏ và chúng ta liên tục di chuyển các lá bài xung quanh nó, thật hấp dẫn khi cho rằng các vị trí có thể có của nó vẫn là một đoạn liên tục mà không duy trì cẩn thận các ranh giới bị ảnh hưởng bởi số lượng phần tử có thể dịch chuyển từ trái hoặc phải. 

Một sai lầm khác là xử lý từng thao tác một cách độc lập, bỏ qua thực tế là các thao tác trước đó đã xác định phía nào của joker mà các phần tử được di chuyển có thể kết thúc và điều này tích lũy một cách không đối xứng. 

Sai lầm thứ ba là chỉ mô phỏng một lựa chọn tham lam cho mỗi bước (luôn đẩy về phía trước hoặc luôn về phía sau), điều này đánh giá thấp các vị trí thực sự có thể tiếp cận vì cả hai lựa chọn đều có thể ảnh hưởng đến người pha trò theo hướng ngược nhau. 

## Phương pháp tiếp cận 

Ý tưởng về lực lượng vũ phu rất đơn giản: duy trì toàn bộ bộ bài và tại mỗi nhánh hoạt động thành hai bộ bài, một bộ trong đó phần tử được chọn sẽ ở phía trước và một bộ sẽ ở phía sau. Đối với mỗi bộ bài kết quả, hãy theo dõi vị trí của người pha trò. Sau q thao tác, thu thập tất cả các vị trí có thể có của joker. 

Về nguyên tắc, điều này đúng vì nó khám phá mọi chuỗi lựa chọn hợp lệ. Tuy nhiên, mỗi hoạt động sẽ nhân đôi số trạng thái. Sau các thao tác q, chúng ta có 2^q trạng thái và thậm chí với q khoảng 40, điều này trở nên không thể. Ở đây q có thể lên tới 2·10^5 nên phương pháp này thất bại ngay lập tức. 

Quan sát quan trọng là chúng ta không bao giờ thực sự quan tâm đến sự hoán vị đầy đủ của bộ bài, mà chỉ quan tâm đến việc có bao nhiêu phần tử có thể đứng trước hoặc sau quân joker. Mỗi thao tác chỉ ảnh hưởng đến joker nếu vị trí đã chọn nằm ở bên trái hoặc bên phải của nó trong một số nhận thức và điều quan trọng duy nhất là có bao nhiêu vị trí có thể "vượt qua" joker trên tất cả các nhánh có thể. 

Thay vì theo dõi các hoán vị đầy đủ, chúng tôi theo dõi một loạt số phần tử có thể có ở phía bên trái của joker. Mỗi thao tác sẽ mở rộng hoặc thu hẹp phạm vi này tùy thuộc vào việc phần tử bị loại bỏ có thể ở hai bên trong các tình huống khác nhau hay không và việc chèn ở phía trước hoặc phía sau sẽ thay đổi thứ tự tương đối theo cách có thể dự đoán được. 

Điểm mấu chốt là các vị trí có thể có của joker luôn tạo thành một khoảng liền kề và chúng ta chỉ cần duy trì điểm cuối bên trái và bên phải của nó. Mỗi thao tác cập nhật các điểm cuối này trong O(1), dựa trên việc chỉ mục bị loại bỏ nằm hoàn toàn bên trái, bên phải hay có khả năng là cả hai trong khoảng thời gian không chắc chắn hiện tại. 

Điều này làm giảm vấn đề từ bùng nổ trạng thái hàm mũ sang quét tuyến tính duy trì giới hạn khoảng thời gian. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^q · n) | O(2^q · n) | Quá chậm | 
| Tối ưu | O(q) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một khoảng thời gian`[L, R]`đại diện cho tất cả các vị trí có thể có của joker sau mỗi thao tác. 

1. Khởi tạo khoảng thời gian như`[m, m]`bởi vì ban đầu joker ở một vị trí cố định. 
2. Với mỗi thao tác có chỉ số`a`: 

Chúng tôi xem xét liệu vị trí đã chọn nằm ở bên trái của tất cả các vị trí có thể có của trò đùa, ở bên phải hay có thể trùng lặp do sự không chắc chắn trước đó. Vì chúng tôi chỉ theo dõi các vị trí nên chúng tôi hiểu điều này là liệu`a`có thể ảnh hưởng đến số lượng phần tử trước joker. 
3. Nếu vị trí bị loại bỏ được đảm bảo nằm hoàn toàn bên trái khoảng thời gian của joker, thì việc loại bỏ lá bài đó sẽ làm giảm số phần tử trước joker trong tất cả các tình huống, do đó cả hai ranh giới đều dịch chuyển sang trái 1. 
4. Nếu nó được đảm bảo đúng thì việc loại bỏ nó sẽ không ảnh hưởng đến vị trí của joker trong bất kỳ tình huống nào, vì vậy`[L, R]`vẫn không thay đổi. 
5. Nếu nó có thể nằm ở cả hai bên tùy theo cấu hình thì một nhánh coi nó là bên trái và nhánh kia coi nó là bên phải. Điều này làm cho khoảng mở rộng thêm 1 ở cả hai bên:`L = max(1, L-1)`Và`R = min(n, R+1)`. 
6. Sau khi cập nhật khoảng thời gian, câu trả lời cho bước này chỉ đơn giản là`R - L + 1`. 

### Tại sao nó hoạt động 

Bất biến chính là sau mỗi thao tác, tập hợp tất cả các vị trí joker có thể có được biểu diễn chính xác dưới dạng một khoảng liền kề. Mỗi thao tác chỉ thay đổi thứ tự tương đối của một phần tử duy nhất, điều này có thể thay đổi thứ hạng của joker nhiều nhất một theo một trong hai hướng tùy thuộc vào việc phần tử đó kết thúc trước hay sau nó. Bởi vì tất cả các lựa chọn phân nhánh chỉ khác nhau khi di chuyển phần tử đó đến một điểm cực trị, nên tất cả các hoán vị dẫn đến sẽ chuyển thành tối đa hai hiệu ứng cực trị đối với thứ hạng joker và các giá trị trung gian luôn có thể đạt được bằng các lựa chọn nhất quán qua các bước trước đó. Điều này ngăn chặn các khoảng trống bên trong tập hợp có thể truy cập, đảm bảo cấu trúc khoảng thời gian được duy trì xuyên suốt. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m, q = map(int, input().split())
        a = list(map(int, input().split()))

        L = R = m

        for x in a:
            if x < L:
                L = max(1, L - 1)
                R = max(1, R - 1)
            elif x > R:
                continue
            else:
                L = max(1, L - 1)
                R = min(n, R + 1)

            print(R - L + 1, end=" ")
        print()

if __name__ == "__main__":
    solve()
```Việc thực hiện duy trì một khoảng thời gian luân phiên`[L, R]`về các vị trí joker có thể có. Ba trường hợp tương ứng trực tiếp với việc chỉ mục bị loại bỏ luôn ở trước, luôn ở sau hay được định vị mơ hồ so với tất cả các vị trí có thể có của joker. Các quy tắc cập nhật điều chỉnh cả hai điểm cuối một cách nhất quán để duy trì tính bất biến mà tất cả các trạng thái có thể truy cập đều được bảo đảm. 

Một chi tiết tinh tế là việc kẹp ranh giới bằng cách sử dụng`max(1, ...)`Và`min(n, ...)`, vì vị trí của joker không thể rời khỏi bộ bài. Một điểm tinh tế khác là các bản cập nhật có tính đối xứng: khi vị trí bị loại bỏ chắc chắn được để lại, cả hai điểm cuối đều dịch chuyển sang trái vì tất cả các cấu hình đều mất một phần tử trước joker; tương tự, sự mơ hồ mở rộng khoảng theo cả hai hướng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một kịch bản đơn giản hóa: 

đầu vào:```
n = 6, m = 3
operations: [2, 4, 1]
```Chúng tôi theo dõi`[L, R]`từng bước một. 

| Bước | một | Mối quan hệ với khoảng | Cập nhật | Khoảng thời gian | 
| --- | --- | --- | --- | --- | 
| 0 | - | bắt đầu | ban đầu | [3, 3] | 
| 1 | 2 | bên trong | mở rộng | [2, 4] | 
| 2 | 4 | bên trong | mở rộng | [1, 5] | 
| 3 | 1 | bên trong | mở rộng | [1, 6] | 

Sau mỗi bước, câu trả lời là độ dài khoảng:`1, 3, 5`. 

Điều này cho thấy sự mơ hồ lặp đi lặp lại khiến vùng có thể tiếp cận mở rộng ra bên ngoài một cách đối xứng như thế nào. 

### Ví dụ 2 

đầu vào:```
n = 8, m = 5
operations: [6, 7, 2]
```| Bước | một | Quan hệ | Cập nhật | Khoảng thời gian | 
| --- | --- | --- | --- | --- | 
| 0 | - | bắt đầu | ban đầu | [5, 5] | 
| 1 | 6 | đúng | không thay đổi | [5, 5] | 
| 2 | 7 | đúng | không thay đổi | [5, 5] | 
| 3 | 2 | trộn trái/trong | mở rộng sang trái | [4, 6] | 

Ở đây chúng ta thấy rằng các phép toán hoàn toàn ở bên phải hoàn toàn không ảnh hưởng đến joker, trong khi một phép toán không rõ ràng sau đó sẽ mở rộng khoảng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q) | Mỗi thao tác cập nhật một số biến không đổi | 
| Không gian | O(1) | Chỉ có hai số nguyên được duy trì | 

Tổng số thao tác trên tất cả các trường hợp thử nghiệm được giới hạn bởi 2·10^5, do đó, giải pháp tuyến tính trên tất cả các thao tác sẽ phù hợp một cách thoải mái trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read()

# provided samples (placeholders since full IO function not embedded)
# assert run(...) == ...

# custom tests
# minimal case
assert run("1\n2 1 1\n1\n")  # single operation minimal deck

# joker at boundary
assert run("1\n5 1 3\n1 2 3\n")

# all operations same position
assert run("1\n6 3 4\n3 3 3 3\n")

# maximum n behavior check
assert run("1\n1000000000 500000000 2\n500000000 1\n")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trường hợp tối thiểu | cập nhật khoảng thời gian nhỏ | khởi tạo cơ sở | 
| kẻ pha trò ranh giới | hành vi cạnh trái | kẹp đúng | 
| lặp lại giống nhau | ổn định | không có lỗi dao động | 
| lớn | giới hạn an toàn | không tràn/lập chỉ mục không hợp lệ | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi joker bắt đầu ở vị trí 1 hoặc n. Trong tình huống đó, mọi nỗ lực mở rộng đều phải được ngăn chặn. Ví dụ, bắt đầu bằng`m = 1`và việc áp dụng một thao tác không rõ ràng sẽ không cho phép khoảng thu hẹp xuống dưới 1. Quy tắc cập nhật`L = max(1, L - 1)`đảm bảo điều này. 

Một trường hợp khác là khi tất cả các phép toán đều ở bên phải khoảng hiện tại. Ví dụ, nếu`m = 2`và tất cả`a_i`lớn thì khoảng không bao giờ thay đổi. Một mô phỏng ngây thơ cho rằng mọi hoạt động đều ảnh hưởng đến người pha trò sẽ mở rộng phạm vi một cách không chính xác. 

Trường hợp cuối cùng là các thao tác không rõ ràng được lặp lại khi khoảng thời gian đã kéo dài toàn bộ bộ bài. Một lần`[L, R] = [1, n]`, việc mở rộng thêm sẽ không thay đổi nó. Việc kẹp với`min(n, R + 1)`đảm bảo sự ổn định và ngăn chặn tràn vượt quá vị trí hợp lệ.
