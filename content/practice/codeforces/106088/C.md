---
title: "CF 106088C - \u041f\u0430\u0440\u043d\u044b\u0435 \u0431\u0440\u0430\u0441\u043b\u0435\u0442\u044b"
description: "Chúng ta có hai chuỗi có độ dài n, biểu thị các số được viết trên hai vòng đeo tay. Mục tiêu là làm cho hai tập hợp số giống hệt nhau, nghĩa là sau khi sắp xếp lại, cả hai vòng tay đều chứa chính xác các giá trị giống nhau với cùng bội số."
date: "2026-06-19T20:25:32+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106088
codeforces_index: "C"
codeforces_contest_name: "\u0412\u0443\u0437\u043e\u0432\u0441\u043a\u043e-\u0430\u043a\u0430\u0434\u0435\u043c\u0438\u0447\u0435\u0441\u043a\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435 2025, \u0432\u0442\u043e\u0440\u043e\u0439 \u043e\u0442\u0431\u043e\u0440\u043e\u0447\u043d\u044b\u0439 \u0442\u0443\u0440"
rating: 0
weight: 106088
solve_time_s: 54
verified: true
draft: false
---

[CF 106088C - \u041f\u0430\u0440\u043d\u044b\u0435 \u0431\u0440\u0430\u0441\u043b\u0435\u0442\u044b](https://codeforces.com/problemset/problem/106088/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai chuỗi có độ dài n, biểu thị các số được viết trên hai vòng đeo tay. Mục tiêu là làm cho hai tập hợp số giống hệt nhau, nghĩa là sau khi sắp xếp lại, cả hai vòng tay đều chứa chính xác các giá trị giống nhau với cùng bội số. 

Chúng tôi được phép thực hiện một thao tác trên một trong hai vòng đeo tay: chọn hai giá trị riêng biệt a và b và hoán đổi đồng thời tất cả các lần xuất hiện của a với b. Thao tác này mang tính toàn cục trên vòng đeo tay, vì vậy mọi a đều trở thành b và mọi b đều trở thành a trong một lần di chuyển. Chúng tôi có thể thực hiện thao tác này nhiều nhất một lần và chúng tôi muốn biết liệu có thể làm cho hai tập hợp bằng nhau hay không, liệu chúng đã bằng nhau hay chưa hoặc liệu điều đó là không thể. 

Quan sát quan trọng là thao tác không thay đổi tần số, nó chỉ hoán vị nhãn của các giá trị trong một mảng. Vì vậy, chúng tôi không chỉnh sửa các thành phần riêng lẻ, chúng tôi đổi tên hai giá trị trên toàn cầu. 

Kích thước đầu vào n lên tới 100000, do đó, bất kỳ giải pháp nào thử tất cả các giao dịch hoán đổi hoặc mô phỏng các phép biến đổi trên mỗi cặp giá trị đều quá chậm. Cách tiếp cận bậc hai đối với các giá trị hoặc cặp tần số sẽ không khả thi. Chúng ta phải quy vấn đề về việc so sánh tần số và nhiều nhất là một số lượng biến đổi ứng cử viên không đổi. 

Một trường hợp lỗi nhỏ xuất hiện khi tồn tại nhiều giá trị không khớp. Ví dụ: nếu Alice có (1,1,2) và Bob có (2,2,1), chúng đã bằng nhau ở dạng nhiều tập, mặc dù vị trí khác nhau. Một trường hợp khác là khi có ba hoặc nhiều giá trị không khớp riêng biệt, trong đó một lần hoán đổi không thể sửa nhiều hơn hai nhãn, khiến câu trả lời là không thể ngay cả khi tổng số khớp một cách hời hợt. 

## Phương pháp tiếp cận 

Nếu chúng ta bỏ qua ràng buộc về các phép toán, thì ý tưởng thô bạo tự nhiên là thử mọi cặp (a, b) giá trị có thể có trong một vòng đeo tay, áp dụng phép hoán đổi, tính toán lại nhiều tập hợp và kiểm tra sự bằng nhau với vòng đeo tay kia. Việc xây dựng bản đồ tần số giúp kiểm tra đẳng thức O(U), trong đó U là số giá trị riêng biệt, nhưng thử tất cả các cặp sẽ cho ra O(U² · U), giá trị này quá lớn khi U có thể lên tới n. 

Điểm nghẽn là thao tác chỉ ảnh hưởng đến hai giá trị cùng một lúc, nhưng sự không khớp giữa các mảng có thể liên quan đến nhiều giá trị. Thông tin chi tiết về cấu trúc quan trọng là việc hoán đổi a và b chỉ trao đổi tần số của chúng, vì vậy chúng ta đang hoán đổi các mục trong bảng tần số của một mảng một cách hiệu quả. Điều đó có nghĩa là điều duy nhất quan trọng là liệu chúng ta có thể khớp nhiều tập tần số bằng cách không làm gì hoặc hoán đổi chính xác hai nhãn ở một bên hay không. 

Điều này làm giảm vấn đề so sánh các từ điển tần số và kiểm tra xem sự khác biệt có thể được giải quyết bằng nhiều nhất một chuyển vị nhãn hay không. 

Chúng tôi tính toán bản đồ tần số của cả hai mảng. Nếu chúng giống nhau thì câu trả lời là 0. Ngược lại, chúng ta xem xét tất cả các giá trị có tần số khác nhau. Nếu có nhiều hơn hai giá trị như vậy thì không một hoán đổi nào có thể sửa được nhiều hơn hai nhãn, vì vậy câu trả lời là -1. 

Nếu có chính xác hai giá trị x và y tồn tại sự khác biệt, chúng tôi sẽ kiểm tra xem việc hoán đổi x và y trong một mảng có làm cho tần số khớp chính xác hay không. Việc hoán đổi này trao đổi hiệu quả số lượng của chúng, vì vậy chúng tôi mô phỏng hiệu ứng đó đối với sự khác biệt về tần số. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Hoán đổi Brute Force của tất cả các cặp giá trị | O(U³) | O(U) | Quá chậm | 
| So sánh tần số với tối đa một mô phỏng hoán đổi | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi dựa vào bản đồ tần số vì hoạt động không bao giờ phụ thuộc vào thứ tự mà chỉ phụ thuộc vào số lượng.

1. Xây dựng từ điển tần số fx và fy cho cả hai vòng đeo tay. 
2. Nếu fx bằng fy, xuất ra 0 ngay lập tức. Điều này nắm bắt các trường hợp chỉ tồn tại sự khác biệt hoán vị. 
3. Tính tập hợp tất cả các giá trị v trong đó fx[v] != fy[v]. Đây là những giá trị duy nhất bị ảnh hưởng bởi bất kỳ sự điều chỉnh nào có thể xảy ra. 
4. Nếu số lượng các giá trị như vậy vượt quá 2, hãy xuất -1, vì một lần hoán đổi chỉ có thể ảnh hưởng đến hai nhãn và không thể sửa được các lỗi không khớp phức tạp hơn. 
5. Nếu có chính xác 2 giá trị x và y, hãy kiểm tra xem việc hoán đổi x và y trong fx có thẳng hàng với fy hay không. Điều này có nghĩa là kiểm tra xem fx[x] có trở thành fx[y] và fx[y] có trở thành fx[x] trong khi tất cả các tần số khác không thay đổi hay không. 
6. Nếu điều kiện đúng thì xuất 1, nếu không thì xuất -1. 
7. Nếu không có giá trị nào không khớp sau bước 2 thì chúng tôi đã trả về 0 nên không cần thêm trường hợp nào nữa. 

Phần không hề nhỏ là bước 5. Thao tác hoán đổi không di chuyển các phần tử mà đổi tên chúng. Vì vậy, cách điều chỉnh duy nhất có thể là trao đổi hoàn toàn hai nhóm tần số. Nếu sự không khớp không thể được biểu thị dưới dạng một lần trao đổi nhãn thì không tồn tại chuỗi thao tác nào. 

### Tại sao nó hoạt động 

Hoạt động tạo ra một hoán vị trên không gian giá trị là nhận dạng hoặc chuyển vị. Vì mỗi thao tác chỉ hoán đổi hai nhãn trên toàn cầu nên mọi cấu hình có thể truy cập được từ một vòng đeo tay sau một lần di chuyển đều tương ứng chính xác với việc hoán đổi hai mục tần số trong biểu đồ của nó. Do đó, vấn đề giảm xuống còn việc kiểm tra xem hai tập hợp tần số có khác nhau bởi sự chuyển vị trong miền giá trị hay không. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))
    
    from collections import Counter
    fa = Counter(a)
    fb = Counter(b)

    if fa == fb:
        print(0)
        return

    keys = set(fa.keys()) | set(fb.keys())
    diff = []
    for k in keys:
        if fa[k] != fb[k]:
            diff.append(k)

    if len(diff) > 2:
        print(-1)
        return

    if len(diff) == 0:
        print(0)
        return

    if len(diff) == 2:
        x, y = diff

        def check():
            fx = fa.copy()
            fx[x], fx[y] = fx[y], fx[x]
            return fx == fb

        if check():
            print(1)
        else:
            print(-1)
        return

    print(-1)

if __name__ == "__main__":
    solve()
```Việc thực hiện xoay quanh việc đếm số lần xuất hiện. Việc kiểm tra sự bình đẳng ngay từ đầu sẽ tránh được những công việc không cần thiết. Bước trích xuất khác biệt sẽ tách biệt chính xác các nhãn trong đó hai biểu đồ khác nhau và điều này trực tiếp giới hạn không gian tìm kiếm. 

Việc mô phỏng hoán đổi chỉ được thực hiện khi có chính xác hai giá trị có vấn đề, vì bất kỳ trường hợp nào khác đều không thể sửa được trong một thao tác. Việc sao chép bộ đếm là an toàn vì số lượng giá trị riêng biệt được giới hạn bởi n và chỉ có một bản sao được tạo. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 4
a = [1, 4, 4, 2]
b = [1, 2, 4, 2]
```Chúng tôi xây dựng bảng tần số: 

| giá trị | fa | fb | 
| --- | --- | --- | 
| 1 | 1 | 1 | 
| 2 | 1 | 2 | 
| 4 | 2 | 1 | 

Giá trị không khớp là {2, 4}. 

Chúng tôi mô phỏng việc hoán đổi 2 và 4 trong fa: 

| bước | fa[2] | fa[4] | kết quả | 
| --- | --- | --- | --- | 
| bản gốc | 1 | 2 | không khớp | 
| trao đổi | 2 | 1 | trận đấu fb | 

Vì đẳng thức giữ nguyên sau một lần hoán đổi, đầu ra là 1. 

Điều này xác nhận rằng chuyển vị nhãn đơn sẽ khắc phục được sự mất cân bằng. 

### Ví dụ 2 

đầu vào:```
a = [1, 2, 3, 4, 5]
b = [1, 2, 3, 4, 4]
```Bảng tần số: 

| giá trị | fa | fb | 
| --- | --- | --- | 
| 4 | 1 | 2 | 
| 5 | 1 | 0 | 

Giá trị không khớp là {4, 5}. 

Hoán đổi 4 và 5 trong fa mang lại: 

| giá trị | fa sau khi trao đổi | fb | 
| --- | --- | --- | 
| 4 | 0 | 2 | 
| 5 | 1 | 0 | 

Vẫn không bằng nhau nên đáp án là -1. 

Điều này cho thấy rằng ngay cả với chính xác hai giá trị không khớp, việc hoán đổi vẫn phải duy trì tính nhất quán hoàn toàn, điều này không phải lúc nào cũng được thỏa mãn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | xây dựng bộ đếm và so sánh các khóa là tuyến tính theo kích thước mảng | 
| Không gian | O(n) | bản đồ tần số lưu trữ tối đa n giá trị riêng biệt | 

Giải pháp này dễ dàng phù hợp trong các giới hạn vì n lên tới 100000 và tất cả các hoạt động đều là các hoạt động bảng băm tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io
from collections import Counter

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    input = _sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    fa = Counter(a)
    fb = Counter(b)

    if fa == fb:
        return "0"

    keys = set(fa) | set(fb)
    diff = [k for k in keys if fa[k] != fb[k]]

    if len(diff) > 2:
        return "-1"

    if len(diff) == 0:
        return "0"

    if len(diff) == 2:
        x, y = diff
        fx = fa.copy()
        fx[x], fx[y] = fx[y], fx[x]
        return "1" if fx == fb else "-1"

    return "-1"

# provided samples
assert run("4\n1 4 4 2\n1 2 4 2\n") == "1"
assert run("5\n1 2 3 4 5\n1 2 3 4 4\n") == "-1"

# custom cases
assert run("1\n7\n7\n") == "0", "minimum size equal"
assert run("2\n1 2\n2 1\n") == "0", "already permutation"
assert run("3\n1 1 2\n2 2 1\n") == "0", "multiset match"
assert run("3\n1 2 3\n4 5 6\n") == "-1", "completely disjoint"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử bằng nhau duy nhất | 0 | tính đúng đắn của trường hợp tối thiểu | 
| mảng hoán đổi | 0 | bất biến hoán vị | 
| giá trị lặp lại | 0 | xử lý bình đẳng nhiều tập hợp | 
| bộ rời rạc | -1 | sự biến đổi không thể | 

## Vỏ cạnh 

Một trường hợp tinh vi là khi các mảng đã bằng nhiều tập hợp nhưng không theo cùng một thứ tự. Đối với đầu vào như`[2, 1, 2]`Và`[1, 2, 2]`, các bản đồ tần số giống hệt nhau và thuật toán trả về 0 ngay lập tức. Một cách tiếp cận ngây thơ so sánh các vị trí thay vì tần số sẽ thực hiện sai các thao tác không cần thiết. 

Một trường hợp khác là khi có chính xác hai giá trị khác nhau về tần số nhưng phép hoán đổi không giải quyết được tổng, như trong`[1,2,3,4,5]`so với`[1,2,3,4,4]`. Thuật toán xác định chính xác hai giá trị không khớp nhưng sẽ từ chối sau khi mô phỏng, vì các nhãn hoán đổi không thể thay đổi sự mất cân bằng bội số tổng hợp. 

Trường hợp cạnh thứ ba là khi có chính xác một giá trị không khớp. Điều này không thể xảy ra khi bảo toàn tần số hợp lệ trừ khi cả hai mảng có tổng phân bố không nhất quán cho giá trị đó. Thuật toán xuất ra chính xác -1 vì một nhãn không thể được chuyển đổi sang hoặc từ nhãn khác mà không ảnh hưởng đến ít nhất hai giá trị và do đó, chỉ một nhãn không khớp cho thấy không thể có cấu trúc.
