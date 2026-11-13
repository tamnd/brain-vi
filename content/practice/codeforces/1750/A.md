---
title: "CF 1750A - Sắp xếp gián tiếp"
description: "Chúng ta được cho một hoán vị nhỏ, nghĩa là mỗi số từ 1 đến n xuất hiện đúng một lần. Nhiệm vụ là xác định xem liệu chúng ta có thể chuyển đổi mảng này thành thứ tự được sắp xếp hay không bằng cách sử dụng một thao tác rất cụ thể bao gồm ba chỉ số i < j < k."
date: "2026-06-09T15:04:58+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1750
codeforces_index: "A"
codeforces_contest_name: "CodeTON Round 3 (Div. 1 + Div. 2, Rated, Prizes!)"
rating: 800
weight: 1750
solve_time_s: 121
verified: true
draft: false
---

[CF 1750A - Sắp xếp gián tiếp](https://codeforces.com/problemset/problem/1750/A) 

**Đánh giá:** 800 
**Tags:** thuật toán xây dựng, triển khai, toán học 
**Thời gian giải:** 2m 1s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị nhỏ, nghĩa là mỗi số từ 1 đến n xuất hiện đúng một lần. Nhiệm vụ là xác định xem liệu chúng ta có thể chuyển đổi mảng này thành thứ tự được sắp xếp hay không bằng cách sử dụng một thao tác rất cụ thể bao gồm ba chỉ số i < j < k. 

Mỗi thao tác thực hiện theo một trong hai cách tùy thuộc vào sự so sánh giữa các giá trị tại vị trí i và k. Nếu giá trị tại i lớn hơn giá trị tại k, chúng ta được phép tăng giá trị tại i bằng cách thêm a[j] vào nó. Ngược lại, chúng ta có thể hoán đổi các phần tử ở vị trí j và k. 

Mục đích không phải là mô phỏng một thuật toán sắp xếp cố định mà là để quyết định xem liệu có thể thực hiện được bằng cách sử dụng lặp lại thao tác này để cuối cùng đạt đến hoán vị được sắp xếp hay không. 

Các ràng buộc là cực kỳ nhỏ, với n nhiều nhất là 10, điều này cho thấy rằng thoạt nhìn thì việc tìm kiếm trạng thái bạo lực có vẻ hấp dẫn. Tuy nhiên, hệ số phân nhánh của hoạt động là lớn vì mỗi bộ ba chỉ số đều là một động thái tiềm năng và các giá trị có thể thay đổi, không chỉ vị trí. Ngay cả trong một n nhỏ như vậy, BFS ngây thơ trên các trạng thái đầy đủ sẽ trở nên lộn xộn vì các giá trị không còn là hoán vị giới hạn sau loại hoạt động đầu tiên. 

Một trường hợp phức tạp quan trọng là thao tác đầu tiên thay đổi giá trị, phá vỡ cấu trúc hoán vị. Ví dụ: bắt đầu từ một hoán vị hợp lệ, việc áp dụng bước “cộng” có thể tạo ra các giá trị lớn hơn n, điều này khiến việc theo dõi trạng thái khó hơn đáng kể nếu chúng ta cố gắng mô phỏng trực tiếp. 

Một tình huống quan trọng khác là khi các giao dịch hoán đổi dường như có sẵn nhưng thực tế lại bị chặn bởi điều kiện trên a[i] và a[k]. Ví dụ: nếu phần tử nhỏ nhất không được định vị chính xác, nó có thể ngăn cản mọi sự sắp xếp lại hữu ích mặc dù các hoán đổi tồn tại về mặt cú pháp. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ coi mỗi cấu hình mảng là một trạng thái và khám phá tất cả các hoạt động có thể có. Từ một trạng thái nhất định, chúng tôi sẽ thử mọi bộ ba i < j < k, tạo ra một hoán đổi hoặc thay đổi giá trị và tiếp tục tìm kiếm cho đến khi chúng tôi đạt được mảng đã sắp xếp hoặc các khả năng cạn kiệt. 

Về nguyên tắc, điều này đúng vì nó mô hình hóa các quy tắc một cách chính xác. Tuy nhiên, số lượng trạng thái bùng nổ ngay lập tức. Ngay cả khi bỏ qua sự tăng trưởng giá trị từ việc bổ sung, vẫn có n! hoán vị và mỗi trạng thái phân nhánh thành các chuyển tiếp O(n^3). Khi các giá trị thay đổi, không gian trạng thái thậm chí không còn bị giới hạn bởi các hoán vị, khiến cho việc tìm kiếm về cơ bản không được kiểm soát. 

Quan sát quan trọng là hoạt động này có một ràng buộc cấu trúc rất mạnh ẩn bên trong nó. Thao tác hoán đổi chỉ được kích hoạt khi a[i] ≤ a[k]. Điều kiện duy nhất này xác định liệu chúng ta có thể sắp xếp lại các phần tử hay không. Nếu tồn tại một vị trí luôn thỏa mãn điều kiện này so với tất cả các giá trị khác, thì nó sẽ trở thành một yếu tố hỗ trợ phổ biến cho các giao dịch hoán đổi. 

Điều này dẫn đến cái nhìn sâu sắc quan trọng: giá trị 1 đóng một vai trò đặc biệt. Vì tất cả các giá trị đều dương và khác biệt nên mọi so sánh với 1 sẽ thỏa mãn a[i] ≤ a[k] bất cứ khi nào a[i] = 1. Nếu chúng ta có thể đặt 1 ở chỉ mục đầu tiên, thì nó có thể đóng vai trò là người hỗ trợ lâu dài cho việc hoán đổi giữa tất cả các vị trí sau này, cho phép sắp xếp lại hậu tố tùy ý một cách hiệu quả. 

Khi điều này xảy ra, việc sắp xếp trở nên đơn giản vì chúng ta có thể mô phỏng các giao dịch hoán đổi liền kề ở bất kỳ đâu trong mảng bằng thao tác ba. 

Do đó, toàn bộ vấn đề tập trung vào việc kiểm tra xem liệu chúng ta đã có “mỏ neo cho phép hoán đổi” này ngay từ đầu hay chưa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tìm kiếm vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Quan sát mỏ neo | O(n) mỗi lần kiểm tra | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Ý tưởng cốt lõi 

Chúng tôi kiểm tra xem phần tử đầu tiên của mảng có bằng 1 hay không. 

### bước

1. Đọc mảng cho test case hiện tại. 
2. Kiểm tra giá trị tại vị trí 1 (vị trí 0 được lập chỉ mục 0). 
3. Nếu bằng 1, xuất ra "Có". 
4. Ngược lại, xuất ra "No". 

Lý do đằng sau việc kiểm tra này là chỉ khi 1 ở vị trí đầu tiên, nó mới có thể được sử dụng làm phần tử trợ giúp cố định trong mọi thao tác ba. Nếu không có điều này, các hoạt động hoán đổi sẽ bị hạn chế bởi các so sánh liên quan đến các giá trị lớn hơn, ngăn cản việc sắp xếp lại hoàn toàn. 

### Tại sao nó hoạt động 

Thao tác này chỉ cho phép hoán đổi j và k khi tồn tại i ở bên trái sao cho a[i] ≤ a[k]. Nếu phần tử đầu tiên là 1, việc chọn i = 1 đảm bảo điều kiện này luôn đúng với mọi k, vì 1 là giá trị nhỏ nhất có thể. Điều này có nghĩa là bất kỳ cặp (j, k) nào cũng có thể được hoán đổi tự do, do đó mảng có thể được hoán vị tùy ý theo thứ tự được sắp xếp. 

Nếu 1 không ở vị trí đầu tiên, thì không có cách nào để làm cho nó hoạt động như một điểm xoay tối thiểu chung cho các giao dịch hoán đổi và hạn chế về i ngăn cản chúng ta tự do sắp xếp lại các phần tử. Mảng bị mắc kẹt trong một cấu trúc không thể đảm bảo việc sắp xếp đầy đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        if a[0] == 1:
            print("Yes")
        else:
            print("No")

if __name__ == "__main__":
    solve()
```Giải pháp đọc từng trường hợp kiểm thử một cách độc lập và thực hiện kiểm tra liên tục theo thời gian cho mỗi trường hợp. Điểm tinh tế duy nhất là lập chỉ mục: chúng tôi trực tiếp kiểm tra phần tử đầu tiên vì đó là vị trí duy nhất quan trọng để xác định liệu có thể hoán đổi không hạn chế hay không. 

Không cần mô phỏng hoạt động vì quyết định giảm hoàn toàn về vị trí của giá trị 1. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 2 3
1 3 2
2 1 3
```| Mảng | Yếu tố đầu tiên | Quyết định | Đầu ra | 
| --- | --- | --- | --- | 
| [1,2,3] | 1 | tồn tại mỏ neo có khả năng trao đổi | Có | 
| [1,3,2] | 1 | có thể sắp xếp lại toàn bộ | Có | 
| [2,1,3] | 2 | không có mỏ neo phổ quát | Không | 

Khi phần tử đầu tiên là 1, chúng ta có thể tự do hoán đổi bất kỳ cặp nào trong hậu tố, do đó, ngay cả một hoán vị chưa sắp xếp như [1,3,2] cũng có thể được sửa trong một lần di chuyển. 

### Ví dụ 2 

đầu vào:```
4
5 3 4 7 6 2 1
7 6 5 4 3 2 1
1 2 6 7 4 3 5
2 1 3 4 5
```| Mảng | Yếu tố đầu tiên | Quyết định | Đầu ra | 
| --- | --- | --- | --- | 
| [5,3,4,7,6,2,1] | 5 | không có mỏ neo | Không | 
| [7,6,5,4,3,2,1] | 7 | không có mỏ neo | Không | 
| [1,2,6,7,4,3,5] | 1 | tự do trao đổi hoàn toàn | Có | 
| [2,1,3,4,5] | 2 | không có mỏ neo | Không | 

Ví dụ này nhấn mạnh rằng ngay cả những hoán vị có vẻ ngoài có khả năng sắp xếp cao cũng không thể sửa được trừ khi phần tử đầu tiên là 1. Trường hợp thứ ba chứng minh cách có 1 ở phía trước sẽ mở ra toàn quyền kiểm soát mảng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Một lần kiểm tra liên tục cho mỗi trường hợp thử nghiệm | 
| Không gian | O(1) | Chỉ lưu trữ mảng đầu vào | 

Các ràng buộc cho phép tối đa 5000 trường hợp thử nghiệm, nhưng mỗi trường hợp được xử lý trong thời gian không đổi, do đó giải pháp phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    input = sys.stdin.readline
    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        out.append("Yes" if a[0] == 1 else "No")
    return "\n".join(out)

# provided samples
assert run("""7
3
1 2 3
3
1 3 2
7
5 3 4 7 6 2 1
7
7 6 5 4 3 2 1
5
2 1 4 5 3
5
2 1 3 4 5
7
1 2 6 7 4 3 5
""") == """Yes
Yes
No
No
No
No
Yes"""

# custom cases
assert run("""3
3
2 3 1
3
1 2 3
4
1 4 3 2
""") == """No
Yes
Yes"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| hoán vị bắt đầu bằng không 1 | Không | yêu cầu neo là cần thiết | 
| đã được sắp xếp | Có | trường hợp nhận dạng | 
| 1 theo sau là rối loạn | Có | trao đổi tự do đủ | 

## Vỏ cạnh 

Nếu phần tử nhỏ nhất không ở vị trí đầu tiên, mặc dù nó tồn tại ở đâu đó trong mảng, nó không thể đóng vai trò là yếu tố hỗ trợ hoán đổi toàn cục. Điều kiện trên i yêu cầu i < j < k, vì vậy trục xoay phải luôn nằm hoàn toàn bên trái mọi thao tác hoán đổi. Chỉ khi 1 bắt đầu ở chỉ số 1, nó mới có thể đóng vai trò là điểm xoay cho tất cả các cặp khác một cách nhất quán. 

Ví dụ, trong`[2,1,3,4,5]`, giá trị 1 tồn tại nhưng không thể được sử dụng như một trợ giúp chung vì nó không có sẵn ở bên trái của tất cả các vị trí. Thuật toán đưa ra kết quả "Không" một cách chính xác, phù hợp với thực tế là không có chuỗi thao tác nào có thể mở khóa hoàn toàn việc sắp xếp lại tùy ý.
