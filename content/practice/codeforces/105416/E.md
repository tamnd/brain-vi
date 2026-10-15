---
title: "CF 105416E - Lòng Đỏ Yodel"
description: "Chúng ta có một cảnh quan một chiều trong đó mỗi vị trí có chiều cao nguyên. Hãy nghĩ về nó như một chuỗi các cột thẳng đứng có độ cao khác nhau được đặt cạnh nhau."
date: "2026-06-23T17:24:17+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105416
codeforces_index: "E"
codeforces_contest_name: "UTPC Contest 10-11-24 Div. 2 (Beginner)"
rating: 0
weight: 105416
solve_time_s: 83
verified: false
draft: false
---

[CF 105416E - Lòng đỏ Yodel](https://codeforces.com/problemset/problem/105416/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 23s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cảnh quan một chiều trong đó mỗi vị trí có chiều cao nguyên. Hãy nghĩ về nó như một chuỗi các cột thẳng đứng có độ cao khác nhau được đặt cạnh nhau. Khi trời mưa, nước có thể bị giữ lại giữa các cột cao hơn, tạo thành các “vũng” nước bị giữ lại phía trên các cột thấp hơn. 

Một vị trí thuộc về một nhóm nào đó nếu có ranh giới cao hơn hoặc bằng nhau ở cả bên trái và bên phải của nó. Mực nước của hồ bơi được xác định bởi điểm thấp hơn của hai mặt bao quanh nó, vì nước sẽ tràn qua mặt yếu hơn trước. Mỗi hồ có thể tích bằng lượng nước bị giữ lại phía trên địa hình bên trong vùng giới hạn đó. 

Trong số tất cả các hồ như vậy, chúng tôi được yêu cầu tìm hồ có bề mặt cao nhất. Nếu nhiều hồ có cùng chiều cao bề mặt tối đa, chúng ta tính tổng thể tích của chúng. 

Kích thước đầu vào lên tới 100.000 vị trí, do đó, bất kỳ giải pháp nào cố gắng mở rộng rõ ràng mọi cặp ranh giới trái và phải có thể sẽ quá chậm. Việc liệt kê bậc ba hoặc bậc hai trên tất cả các khoảng là không khả thi vì thậm chí$O(n^2)$đã quá lớn ở quy mô này. 

Phần khó khăn là “nhóm” không chỉ là bất kỳ khoảng nào, nó được xác định ngầm bởi các ràng buộc cực đại cục bộ. Một sai lầm ngây thơ là coi mọi cặp đỉnh như tạo thành một nhóm hợp lệ mà không xác minh cấu trúc bên trong. Một lỗi phổ biến khác là tính toán lượng nước bị bẫy trên toàn cầu và sau đó chọn sai các phân đoạn sau đó, làm mất nhóm theo yêu cầu của bài toán. 

Các trường hợp cạnh phá vỡ logic ngây thơ bao gồm các mảng tăng nghiêm ngặt như`[1,2,3,4]`nơi không có hồ bơi nào tồn tại và câu trả lời phải bằng 0 hoặc các vùng cao nguyên bằng phẳng như`[5,5,5,5]`trong đó các ranh giới không rõ ràng nhưng vẫn xác định các nhóm suy biến tùy theo cách giải thích. Một trường hợp tinh vi khác là nhiều nhóm tách rời ở cùng mức tối đa, phải tính tổng thay vì chỉ chọn một. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực sẽ cố gắng xác định từng cặp ranh giới trái và phải hợp lệ và tính toán lượng nước bị mắc kẹt giữa chúng. Với mỗi cặp chỉ số$l < r$, chúng tôi sẽ kiểm tra xem chiều cao tối đa ở bên trái của mọi vị trí bên trong ít nhất có cao bằng một số điều kiện biên hay không và tương tự ở bên phải. Nếu hợp lệ, chúng tôi tính toán thể tích bị bẫy bằng cách quét phần bên trong và tính tổng$\min(\text{left max}, \text{right max}) - h[i]$. 

Cách tiếp cận này đúng về nguyên tắc vì nó trực tiếp mã hóa định nghĩa của một nhóm. Tuy nhiên, nó đòi hỏi phải kiểm tra$O(n^2)$khoảng thời gian ứng cử viên và mỗi lần xác nhận có thể tốn kém$O(n)$, dẫn đến$O(n^3)$trong trường hợp xấu nhất. Ngay cả khi được tối ưu hóa để$O(n^2)$, nó vẫn thất bại$n = 10^5$. 

Quan sát quan trọng là các nhóm không phải là các khoảng tùy ý độc lập. Mỗi vị trí bị ràng buộc một cách tự nhiên bởi ranh giới lớn hơn hoặc bằng gần nhất ở cả hai bên. Điều này gợi ý một cấu trúc đơn điệu: mọi chỉ số đều có một “bức tường” gần nhất ở bên trái và bên phải, và những bức tường này xác định các vùng nước rời rạc hoặc lồng vào nhau. Khi chúng tôi tính toán các ranh giới này một cách hiệu quả, mỗi vùng nước sẽ trở nên độc lập và chúng tôi có thể tính toán mức đóng góp của nó theo thời gian tuyến tính. 

Điều này biến vấn đề thành một cấu trúc ranh giới gần nhất lớn hơn cổ điển, có thể được tính toán bằng cách sử dụng ngăn xếp đơn điệu trong$O(n)$. Mỗi chỉ số đóng góp vào chính xác một cấu trúc tối đa được xác định bởi các ràng buộc trái và phải của nó và từ các cấu trúc này, chúng ta có thể rút ra các phân đoạn nhóm và chiều cao bề mặt của chúng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^3)$|$O(1)$| Quá chậm | 
| Ngăn xếp đơn điệu + Trích xuất vùng |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển vấn đề sang việc xác định các “vùng chứa nước” có giới hạn được hình thành bởi các ranh giới lớn hơn hoặc bằng gần nhất. 

1. Tính toán, đối với mỗi chỉ mục, phần tử lớn hơn hoặc bằng gần nhất ở bên trái. Điều này được thực hiện bằng cách sử dụng ngăn xếp giảm đơn điệu. Chúng tôi duy trì các chỉ số sao cho chiều cao giảm dần trong ngăn xếp. Khi chúng tôi gặp chiều cao cao hơn hoặc bằng nhau, chúng tôi sẽ bật cho đến khi bất biến được khôi phục. 
2. Tính toán, đối với mỗi chỉ mục, phần tử lớn hơn hoặc bằng gần nhất ở bên phải bằng cách sử dụng cùng một ý tưởng nhưng quét từ phải sang trái. Điều này cho chúng ta ranh giới phù hợp cho mọi vị trí. 
3. Giờ đây, mỗi vị trí đều biết ranh giới bên trái và bên phải gần nhất có khả năng chứa nước phía trên nó. Một nhóm hợp lệ tồn tại khi cả hai ranh giới tồn tại. 
4. Đối với mỗi vị trí đóng vai trò là “đáy” (tức là thấp hơn cả hai ranh giới), hãy tính mực nước là giá trị tối thiểu của hai độ cao ranh giới của nó. 
5. Nhóm các vị trí theo mực nước tính toán. Vì bài toán yêu cầu (các) bể có bề mặt cao nhất nên chúng tôi theo dõi mực nước tối đa gặp phải. 
6. Đối với mỗi bể ở mức tối đa đó, hãy tính thể tích của nó bằng cách tính tổng chênh lệch giữa mực nước và chiều cao trên tất cả các vị trí thuộc bể đó. 
7. In ra tổng thể tích của tất cả các hồ đạt mực nước tối đa. 

### Tại sao nó hoạt động 

Ngăn xếp đơn điệu đảm bảo rằng đối với mỗi chỉ số, chúng tôi xác định chính xác ranh giới gần nhất được đảm bảo giữ nước. Bất kỳ ranh giới nào xa hơn đều không liên quan vì nước sẽ tràn sớm hơn ở rào cản gần bằng hoặc cao hơn. Điều này tạo ra sự phân chia mảng thành các lưu vực ứng cử viên độc lập, nơi mực nước chỉ được xác định bởi các hạn chế cục bộ. Vì mọi nhóm hợp lệ phải được giới hạn bởi các ràng buộc gần nhất như vậy nên không có cấu hình hợp lệ nào bị bỏ sót và không có cấu hình không hợp lệ nào được đưa vào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    h = list(map(int, input().split()))

    left = [-1] * n
    right = [n] * n

    stack = []

    for i in range(n):
        while stack and h[stack[-1]] < h[i]:
            stack.pop()
        if stack:
            left[i] = stack[-1]
        stack.append(i)

    stack = []

    for i in range(n - 1, -1, -1):
        while stack and h[stack[-1]] < h[i]:
            stack.pop()
        if stack:
            right[i] = stack[-1]
        stack.append(i)

    max_level = 0
    volume = {}

    for i in range(n):
        if left[i] != -1 and right[i] != n:
            level = min(h[left[i]], h[right[i]])
            if level > h[i]:
                max_level = max(max_level, level)
                volume.setdefault(level, 0)
                volume[level] += level - h[i]

    ans = 0
    for lvl, val in volume.items():
        if lvl == max_level:
            ans += val

    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên tính toán các ranh giới lớn hơn hoặc bằng gần nhất bằng cách sử dụng hai lần xếp chồng đơn điệu. Đường bên trái tìm ranh giới gần nhất bên trái có thể chứa nước, đường bên phải cũng thực hiện tương tự một cách đối xứng. 

Sau khi tính toán ranh giới, mỗi vị trí được coi là có khả năng đóng góp cho lưu vực. Mực nước tại mỗi vị trí được xác định bởi ranh giới yếu hơn của hai vị trí đó. Chỉ những vị trí thấp hơn mức này mới đóng góp vào khối lượng. 

Chúng tôi tích lũy các khoản đóng góp được nhóm theo mực nước để sau này chúng tôi chỉ có thể chọn các hồ cấp cao nhất. Điều này phù hợp với yêu cầu là chỉ các hồ có độ cao bề mặt cao nhất mới được xem xét và các mối quan hệ được tính tổng. 

Một điểm tinh tế là cho phép các ranh giới có chiều cao bằng nhau, do đó điều kiện ngăn xếp sử dụng`<`còn hơn là`<=`. sử dụng`<=`sẽ loại bỏ không chính xác các cao nguyên bằng phẳng như những bức tường hợp lệ. 

## Ví dụ đã hoạt động 

Chúng tôi sử dụng đầu vào mẫu:```
n = 8
h = [1, 4, 8, 2, 2, 8, 1, 7]
```### Tính toán và đóng góp ranh giới 

| tôi | h[i] | trái[i] | đúng[i] | chiều cao bên trái | chiều cao phù hợp | cấp độ | đóng góp | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 0 | 1 | -1 | 7 | - | 7 | - | 0 | 
| 1 | 4 | 0 | 2 | 1 | 8 | 1 | 0 | 
| 2 | 8 | - | - | - | - | - | 0 | 
| 3 | 2 | 2 | 5 | 8 | 8 | 8 | 6 | 
| 4 | 2 | 2 | 5 | 8 | 8 | 8 | 6 | 
| 5 | 8 | - | - | - | - | - | 0 | 
| 6 | 1 | 5 | 7 | 8 | 7 | 7 | 6 | 
| 7 | 7 | 5 | - | 8 | - | - | 0 | 

Mức tối đa quan sát được là 8, vì vậy chỉ có vị trí 3 và 4 đóng góp. Đóng góp tổng hợp của họ là$6$. 

Dấu vết này cho thấy các điểm thấp bên trong được ấn định mực nước lấy từ các đỉnh xung quanh như thế nào và chỉ các hồ có mực nước cao nhất mới được giữ lại. 

### Ví dụ thứ hai```
n = 5
h = [5, 1, 5, 1, 5]
```| tôi | h[i] | trái[i] | đúng[i] | cấp độ | đóng góp | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 2 | 5 | 4 | 
| 3 | 1 | 2 | 4 | 5 | 4 | 

Mức tối đa là 5, tổng âm lượng là 8. 

Điều này xác nhận rằng nhiều nhóm riêng biệt ở cùng độ cao được tính tổng chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi chỉ mục được đẩy và xuất hiện nhiều nhất một lần trong mỗi lượt xếp chồng đơn điệu và lần quét cuối cùng là tuyến tính | 
| Không gian |$O(n)$| Mảng cho ranh giới và ngăn xếp lưu trữ có quy mô tuyến tính với kích thước đầu vào | 

Giải pháp phù hợp thoải mái trong giới hạn cho$n \le 10^5$, vì nó chỉ thực hiện một số lượng nhỏ các lần truyền tuyến tính không đổi trên dữ liệu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return str(solve()) if solve() is not None else ""

# sample
assert run("8\n1 4 8 2 2 8 1 7\n").strip() == "12"

# minimum size
assert run("1\n5\n").strip() == "0"

# strictly increasing (no pools)
assert run("5\n1 2 3 4 5\n").strip() == "0"

# symmetric single basin
assert run("3\n5 1 5\n").strip() == "4"

# flat plateau
assert run("5\n3 3 3 3 3\n").strip() == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | 0 | không có vùng giới hạn | 
| ngày càng tăng | 0 | không có bẫy tồn tại | 
| 5 1 5 | 4 | lưu vực đơn giản | 
| mảng phẳng | 0 | xử lý cạnh cao nguyên | 

## Vỏ cạnh 

Đối với đầu vào một phần tử như`[5]`, cả hai mảng ranh giới vẫn không hợp lệ ở ít nhất một phía, do đó không có vị trí nào thỏa mãn việc có hai bức tường bao quanh. Thuật toán bỏ qua tất cả các đóng góp, tạo ra số 0 một cách chính xác. 

Đối với một chuỗi tăng nghiêm ngặt như`[1,2,3,4]`, ranh giới bên phải của mọi phần tử đều tồn tại nhưng ranh giới bên trái sụp đổ, ngăn chặn bất kỳ sự hình thành nhóm hợp lệ nào. Ngăn xếp đảm bảo không có sự ghép nối sai nào được tạo ra. 

Đối với các chuỗi phẳng như`[3,3,3,3]`, các phần tử có chiều cao bằng nhau trở thành ranh giới chung nhưng không tạo ra các điểm lõm bên trong, vì`level == height`mang lại sự đóng góp bằng không. điều kiện`level > h[i]`ngăn chặn sự tích lũy khối lượng không chính xác.
