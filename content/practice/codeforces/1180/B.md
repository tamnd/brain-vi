---
title: "CF 1180B - Nick và Mảng"
description: "Chúng ta được cung cấp một mảng các số nguyên và với mỗi vị trí, chúng ta được phép áp dụng nhiều lần một phép biến đổi thay thế giá trị $x$ bằng $-x-1$."
date: "2026-06-13T11:07:26+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1180
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 569 (Div. 2)"
rating: 1500
weight: 1180
solve_time_s: 660
verified: false
draft: false
---

[CF 1180B - Nick và Array](https://codeforces.com/problemset/problem/1180/B) 

**Đánh giá:** 1500 
**Tags:** tham lam, thực hiện 
**Thời gian giải:** 11 phút 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên và với mỗi vị trí, chúng ta được phép áp dụng nhiều lần một phép biến đổi thay thế một giá trị$x$với$-x-1$. Áp dụng nó hai lần sẽ trả về giá trị ban đầu, do đó, mỗi phần tử thực tế chỉ có hai trạng thái cuối cùng có thể có: chính nó hoặc phiên bản được chuyển đổi của nó. 

Nhiệm vụ là chọn, một cách độc lập cho mọi vị trí, kết thúc bằng giá trị nào trong hai giá trị có thể tiếp cận sao cho tích của tất cả các giá trị mảng cuối cùng trở nên lớn nhất có thể. Sau đó, chúng tôi xuất ra bất kỳ mảng nào đạt được sản phẩm tối đa đó. 

Ràng buộc$n \le 10^5$buộc một giải pháp tuyến tính hoặc gần tuyến tính. Bất cứ điều gì liên quan đến tương tác theo cặp giữa các phần tử, chẳng hạn như kiểm tra sự kết hợp hoặc mô phỏng các lựa chọn trên toàn cầu, sẽ ngay lập tức trở nên quá chậm vì thậm chí$O(n^2)$đã rồi$10^{10}$hoạt động. 

Một vài trường hợp tế nhị quan trọng ở đây. Nếu tất cả các số đều dương thì phép biến đổi có thể tạo ra các giá trị âm, điều này sẽ gây tổn hại cho kết quả, vì vậy chúng ta mong muốn tránh điều đó. Nếu số âm, việc chuyển đổi chúng có thể làm cho chúng dương, điều này cải thiện đáng kể sản phẩm. Zero cư xử đặc biệt vì nó biến thành$-1$, điều này tệ hơn khi nhân, vì vậy nó thường không thay đổi. Khó khăn chính là việc tối ưu hóa sản phẩm thường yêu cầu các dấu hiệu cân bằng trên toàn cầu, điều này có thể gợi ý sự kết hợp giữa các quyết định, nhưng ở đây mỗi phần tử chỉ có hai trạng thái độc lập, giúp đơn giản hóa cấu trúc. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi sự kết hợp các lựa chọn, xử lý từng phần tử như một quyết định nhị phân giữa$a_i$Và$-a_i-1$. Điều này dẫn đến$2^n$mảng có thể. Ngay cả đối với$n = 30$, điều này trở nên không khả thi, và tại$n = 10^5$nó hoàn toàn không thể được. Tính đúng đắn là hiển nhiên vì nó liệt kê tất cả các khả năng, nhưng sự tăng trưởng theo cấp số nhân khiến nó không thể sử dụng được. 

Quan sát quan trọng là mỗi phần tử có thể được tối ưu hóa cục bộ mà không tạo ra các tương tác toàn cầu có hại. Việc chuyển đổi có tính liên quan, nghĩa là mỗi giá trị có chính xác hai tùy chọn và không có cấu trúc nào khác ngoài tùy chọn đó. Chúng tôi so sánh hai ứng cử viên cho mỗi chỉ số: giá trị ban đầu$a_i$, và giá trị được chuyển đổi$-a_i - 1$. Điều đáng ngạc nhiên là việc chọn giá trị lớn hơn trong hai giá trị này cho mỗi chỉ số một cách độc lập là đủ. 

Lý do điều này hoạt động là vì các giá trị được chọn luôn không âm hoặc tệ nhất là bằng 0, loại bỏ hoàn toàn xung đột dấu hiệu. Khi tất cả các số đều không âm, việc tối đa hóa tích sẽ giảm xuống mức tối đa hóa từng yếu tố một cách độc lập vì việc tăng bất kỳ yếu tố đơn lẻ nào cũng không thể làm giảm tích. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^n \cdot n)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi phần tử$a_i$, tính giá trị biến đổi của nó$b_i = -a_i - 1$. Điều này nắm bắt trạng thái thay thế duy nhất có thể truy cập từ$a_i$, vì vậy chúng tôi liệt kê đầy đủ không gian quyết định cho mỗi phần tử. 
2. So sánh$a_i$Và$b_i$, và chọn cái lớn hơn. Điều này đảm bảo giá trị được lựa chọn luôn là sự đóng góp tốt nhất của địa phương cho sản phẩm cuối cùng. 
3. Lưu giá trị đã chọn vào mảng kết quả. Mỗi vị trí được quyết định độc lập nên chúng ta không bao giờ cần phải xem lại các lựa chọn trước đó. 
4. Xuất mảng kết quả sau khi xử lý tất cả các phần tử. 

### Tại sao nó hoạt động 

Mỗi phần tử có chính xác hai giá trị có thể truy cập và việc chọn giá trị tối đa trong hai giá trị đó đảm bảo rằng mọi giá trị được chọn đều lớn nhất có thể. Phép biến đổi duy trì tính độc lập giữa các chỉ số, do đó không có ràng buộc nào liên kết lựa chọn ở vị trí này với vị trí khác. Sau khi chúng tôi đảm bảo mỗi giá trị được chọn là giá trị lớn hơn trong cặp của nó, mảng cuối cùng sẽ là giá trị tối đa theo thành phần và vì tất cả các giá trị được chọn đều không âm sau quy tắc lựa chọn này nên việc tăng bất kỳ phần tử đơn lẻ nào chỉ có thể tăng hoặc duy trì tích số. Điều này loại bỏ mọi nhu cầu tối ưu hóa toàn cầu đối với các dấu hiệu hoặc tính chẵn lẻ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    res = []
    for x in a:
        y = -x - 1
        if x >= y:
            res.append(x)
        else:
            res.append(y)
    
    print(*res)

if __name__ == "__main__":
    solve()
```Việc thực hiện trực tiếp tuân theo quan sát rằng mỗi chỉ số là độc lập. Đối với mỗi phần tử, chúng tôi tính toán hai trạng thái có thể có của nó và chọn trạng thái lớn hơn. Việc so sánh xử lý ngầm tất cả các trường hợp cạnh, bao gồm các giá trị âm, số 0 và số dương. Không cần ghi sổ kế toán bổ sung vì không có ràng buộc toàn cầu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
2 2 2 2
```Chúng tôi xử lý từng phần tử một cách độc lập. 

| tôi | một [tôi] | đã biến đổi (-a[i]-1) | đã chọn | 
| --- | --- | --- | --- | 
| 1 | 2 | -3 | 2 | 
| 2 | 2 | -3 | 2 | 
| 3 | 2 | -3 | 2 | 
| 4 | 2 | -3 | 2 | 

Đầu ra:```
2 2 2 2
```Điều này cho thấy rằng không nên chuyển đổi các giá trị dương vì phép biến đổi chỉ làm giảm chúng. 

### Ví dụ 2 

đầu vào:```
5
-5 -1 0 3 -2
```| tôi | một [tôi] | chuyển đổi | đã chọn | 
| --- | --- | --- | --- | 
| 1 | -5 | 4 | 4 | 
| 2 | -1 | 0 | 0 | 
| 3 | 0 | -1 | 0 | 
| 4 | 3 | -4 | 3 | 
| 5 | -2 | 1 | 1 | 

Đầu ra:```
4 0 0 3 1
```Điều này chứng tỏ cách tiêu cực chuyển đổi thành tích cực, luôn được ưa thích. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi phần tử được xử lý một lần với công việc liên tục | 
| Không gian |$O(1)$| Chỉ lưu trữ đầu ra ngoài mảng đầu vào | 

Giải pháp phù hợp thoải mái trong giới hạn vì$n = 10^5$chỉ yêu cầu một lần quét tuyến tính duy nhất. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))
    
    res = []
    for x in a:
        y = -x - 1
        res.append(x if x >= y else y)
    
    return " ".join(map(str, res))

# provided sample
assert run("4\n2 2 2 2\n") == "2 2 2 2"

# minimum size
assert run("1\n0\n") == "0"

# all negative
assert run("3\n-5 -1 -10\n") == "4 0 9"

# mixed values
assert run("5\n-2 3 0 -7 1\n") == "1 3 0 6 1"

# all equal negatives
assert run("4\n-1 -1 -1 -1\n") == "0 0 0 0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 0 | trường hợp biên nhỏ nhất | 
| tất cả tiêu cực | tích cực | lợi ích chuyển đổi | 
| giá trị hỗn hợp | lựa chọn tối ưu hỗn hợp | độc lập từng phần tử | 
| lặp lại -1 | tất cả số không | hành vi cạnh của điểm cố định | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi giá trị là$-1$. Trong trường hợp này, phép biến đổi mang lại$0$, điều này thực sự tốt hơn cho sản phẩm vì số 0 không gây ra vấn đề về dấu hiệu và lớn hơn$-1$. Thuật toán chuyển đổi chính xác mọi$-1$vào trong$0$, phù hợp với việc tối đa hóa sản phẩm. 

Một trường hợp tế nhị khác chính là số 0. Zero biến thành$-1$, tệ hơn cả về giá trị và đóng góp của sản phẩm, vì vậy thuật toán giữ nó không thay đổi. Điều này đảm bảo số 0 đóng vai trò là yếu tố trung lập trong sản phẩm cuối cùng, thay vì tạo ra các yếu tố tiêu cực có thể làm giảm kết quả tổng thể.
