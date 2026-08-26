---
title: "CF 104339B - Bốn chuông ấm"
description: "Chúng ta có bốn trọng số nguyên dương và nhiệm vụ là quyết định xem có thể đặt tất cả chúng lên một cái cân để hệ thống có thể cân bằng hoàn hảo hay không."
date: "2026-07-01T18:37:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104339
codeforces_index: "B"
codeforces_contest_name: "FAMCS Olympiad for scholars, Qualification (copy)"
rating: 0
weight: 104339
solve_time_s: 65
verified: true
draft: false
---

[CF 104339B - Bốn chuông ấm](https://codeforces.com/problemset/problem/104339/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có bốn trọng số nguyên dương và nhiệm vụ là quyết định xem có thể đặt tất cả chúng lên một cái cân để hệ thống có thể cân bằng hoàn hảo hay không. Mỗi quả cân có thể được đặt trên đĩa bên trái hoặc đĩa bên phải và mỗi quả cân phải được sử dụng đúng một lần. 

Yêu cầu cốt lõi là tổng trọng lượng trên chảo bên trái phải bằng tổng trọng lượng trên chảo bên phải sau khi gán cả bốn chuông ấm. Không có hạn chế về số lượng vật phẩm được chuyển cho mỗi bên ngoài việc sử dụng tất cả chúng. 

Kích thước đầu vào cực kỳ nhỏ vì luôn có chính xác bốn số. Điều này loại bỏ mọi nhu cầu tối ưu hóa tiệm cận. Ngay cả việc liệt kê đầy đủ tất cả các bài tập cũng có thể thực hiện được vì mỗi chuông ấm có hai lựa chọn trái hoặc phải, chỉ đưa ra 2⁴ = 16 cấu hình. 

Một sai lầm ngây thơ trong loại vấn đề này là cho rằng việc sắp xếp hoặc ghép các giá trị liền kề là đủ. Điều đó không thành công vì sự cân bằng phụ thuộc vào sự kết hợp chứ không phải thứ tự. Ví dụ: với trọng số 1 2 3 4, việc ghép nối tham lam như (1 + 4) và (2 + 3) có hiệu quả, nhưng không có gì đảm bảo rằng việc ghép nối được sắp xếp luôn là cấu trúc có ý nghĩa duy nhất. Tiêu chí đáng tin cậy duy nhất là liệu bất kỳ phân vùng nào của tập hợp thành hai nhóm có tổng bằng nhau hay không. 

Một cạm bẫy tinh vi khác là giả định rằng tổng số tiền phải chia hết cho 2 và ngay lập tức kết luận tính khả thi. Điều kiện đó là cần nhưng chưa đủ. Ví dụ: 1 1 1 3 có tổng bằng 6, chia hết cho 2, nhưng không có tập hợp con nào có tổng bằng 3. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ là thử mọi cách để gán từng chuông ấm ở bên trái hoặc bên phải. Đối với mỗi phép gán, chúng tôi tính toán sự khác biệt giữa hai bên và kiểm tra xem nó có bằng 0 hay không. Vì mỗi mục trong số bốn mục độc lập chọn một bên nên có 2⁴ = 16 khả năng. Mỗi cấu hình yêu cầu công việc liên tục để đánh giá, vì vậy việc này khá nhanh. 

Mặc dù điều này đã tối ưu với n = 4 nhưng nó có tính khái quát kém. Nếu có n mục, cách tiếp cận bạo lực sẽ tăng lên thành 2ⁿ, điều này trở nên không khả thi khi n vượt quá khoảng 25 đến 30 trong các ràng buộc thông thường. Quan sát quan trọng trong vấn đề này là n cố định và rất nhỏ, vì vậy chúng ta không cần tìm kiếm các cải tiến tiệm cận ngoài phép liệt kê hoặc lý luận tập hợp con. 

Chúng ta cũng có thể hiểu bài toán là chia mảng thành hai tập con có tổng bằng nhau. Đây là cách kiểm tra phân vùng tổng tập hợp con cổ điển. Với bốn phần tử, chúng ta có thể chỉ cần kiểm tra tất cả các tập hợp con có kích thước từ 0 đến 4 và xác minh xem có tập hợp con nào có tổng bằng một nửa tổng hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tất cả các bài tập) | O(2⁴) | O(1) | Đã chấp nhận | 
| Tối ưu (liệt kê tập hợp con) | O(2⁴) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta rút gọn bài toán thành việc kiểm tra xem bốn trọng số có thể được chia thành hai nhóm có tổng bằng nhau hay không. 

1. Tính tổng của cả bốn trọng lượng. Nếu tổng này là số lẻ thì dừng ngay lập tức và xuất ra NO. Tổng số lẻ không thể chia thành hai số nguyên bằng nhau. 
2. Đặt mục tiêu bằng một nửa tổng số tiền. Bây giờ chúng ta cần xác định xem liệu tập hợp con nào đó của bốn số có tính tổng chính xác bằng mục tiêu này hay không. 
3. Lặp lại tất cả các tập con của bốn chỉ số. Mỗi tập hợp con đại diện cho việc chọn trọng số nào sẽ chuyển sang chảo bên trái. Các trọng số còn lại ngầm chuyển sang chảo bên phải. 
4. Với mỗi tập hợp con, hãy tính tổng các phần tử đã chọn. Nếu nó bằng mục tiêu, chúng ta có thể kết luận ngay rằng có tồn tại một phân vùng hợp lệ. 
5. Nếu không có tập hợp con nào phù hợp với mục tiêu sau khi kiểm tra tất cả các khả năng, xuất ra NO. 

Lý do đằng sau việc lặp lại các tập hợp con là mọi cách sắp xếp hợp lệ của số dư đều tương ứng với việc chọn một tập hợp con cho một bên. Không có sự trùng lặp hoặc thiếu cấu hình trong phần trình bày này. 

### Tại sao nó hoạt động

Bất kỳ cấu hình hợp lệ nào của thang đo đều tương ứng với việc phân chia bốn trọng lượng thành hai nhóm riêng biệt. Mỗi phân vùng như vậy được biểu diễn chính xác một lần bằng một tập hợp con các chỉ mục. Do đó, việc kiểm tra tất cả các tập hợp con sẽ làm cạn kiệt mọi sự sắp xếp vật lý có thể có. Vì chúng tôi trực tiếp kiểm tra tính bằng nhau của các tổng cho mỗi phân vùng nên kết quả khớp thành công đảm bảo vị trí cân bằng hợp lệ và thất bại trên tất cả các tập hợp con đảm bảo không tồn tại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    p = list(map(int, input().split()))
    total = sum(p)

    if total % 2:
        print("NO")
        return

    target = total // 2
    n = 4

    for mask in range(1 << n):
        s = 0
        for i in range(n):
            if mask & (1 << i):
                s += p[i]
        if s == target:
            print("YES")
            return

    print("NO")

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ đọc bốn số nguyên và tính tổng của chúng. Kiểm tra tính chẵn lẻ tránh việc liệt kê không cần thiết khi tổng là số lẻ. 

Vòng lặp bitmask liệt kê tất cả các tập hợp con của bốn trọng số. Mỗi bit biểu thị liệu một trọng lượng có được đặt trên chảo bên trái hay không. Vòng lặp bên trong tích lũy tổng tập hợp con. Nếu bất kỳ tập hợp con nào khớp với tổng mục tiêu, chúng tôi sẽ xuất ngay CÓ. 

Việc quay lại sớm rất quan trọng vì chúng ta chỉ cần một phân vùng hợp lệ. Nếu không có tập hợp con nào trong số 16 tập hợp con hoạt động thì câu trả lời cuối cùng là KHÔNG. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`7 3 5 5`Tổng số tiền là 20, vì vậy mục tiêu là 10. 

| Mặt nạ | Yếu tố được chọn | Tổng tập hợp con | Trận đấu mục tiêu | 
| --- | --- | --- | --- | 
| 0000 | {} | 0 | Không | 
| 0001 | 7 | 7 | Không | 
| 0010 | 3 | 3 | Không | 
| 0011 | 7,3 | 10 | Có | 

Tập hợp con {7, 3} tạo thành một bên có tổng bằng 10 và {5, 5} còn lại cũng có tổng bằng 10. Điều này xác nhận tồn tại số dư hợp lệ. 

### Ví dụ 2:`7 3 5 6`Tổng số tiền là 21, là số lẻ. Thuật toán ngay lập tức từ chối nó mà không cần liệt kê. 

Điều này chứng tỏ tính hữu ích của việc kiểm tra tính chẵn lẻ, giúp lọc các trường hợp không thể thực hiện được trước khi tìm kiếm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(2⁴) | Chúng tôi liệt kê tất cả các tập hợp con của bốn phần tử | 
| Không gian | O(1) | Chỉ sử dụng các biến không đổi và bộ nhớ đầu vào | 

Hệ số không đổi không đáng kể vì không gian tìm kiếm chỉ có 16 cấu hình. Điều này nằm trong giới hạn ngay cả khi bị hạn chế về thời gian nghiêm ngặt. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import io as sysio

    out = sysio.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

def solve():
    p = list(map(int, sys.stdin.readline().split()))
    total = sum(p)

    if total % 2:
        print("NO")
        return

    target = total // 2
    n = 4

    for mask in range(1 << n):
        s = 0
        for i in range(n):
            if mask & (1 << i):
                s += p[i]
        if s == target:
            print("YES")
            return

    print("NO")

# provided samples
assert run("7 3 5 5") == "YES", "sample 1"
assert run("7 3 5 6") == "NO", "sample 2"

# custom cases
assert run("1 1 1 1") == "YES", "two pairs"
assert run("1 2 3 4") == "YES", "classic partition"
assert run("1 1 1 3") == "NO", "sum even but impossible"
assert run("10 10 10 10") == "YES", "all equal"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1 1 | CÓ | trường hợp không tầm thường nhỏ nhất đối xứng | 
| 1 2 3 4 | CÓ | tồn tại phân vùng không tầm thường | 
| 1 1 1 3 | KHÔNG | tổng chẵn nhưng không có tập hợp con hợp lệ | 
| 10 10 10 10 | CÓ | trường hợp ổn định hoàn toàn bằng nhau | 

## Vỏ cạnh 

Trường hợp một cạnh là khi tất cả các số đều giống hệt nhau, chẳng hạn như`10 10 10 10`. Thuật toán liệt kê các tập hợp con và nhanh chóng tìm ra sự phân chia hợp lệ giống như hai phần tử ở mỗi bên. Việc liệt kê tập hợp con đảm bảo tính chính xác vì nhiều mặt nạ sẽ tạo ra cấu trúc tổng giống nhau, nhưng ít nhất một mặt nạ đạt chính xác một nửa. 

Một trường hợp khác là khi tổng là số lẻ, chẳng hạn như`7 3 5 6`. Thuật toán kết thúc ngay sau khi kiểm tra tính chẵn lẻ. Điều này an toàn vì không tồn tại phân vùng số nguyên thành hai tổng bằng nhau khi tổng là số lẻ, vì vậy việc bỏ qua tìm kiếm tập hợp con sẽ không bỏ sót bất kỳ cấu hình hợp lệ nào. 

Một trường hợp tế nhị hơn là`1 1 1 3`. Mặc dù tổng số là chẵn, việc liệt kê tập hợp con không tìm thấy bất kỳ nhóm nào có tổng bằng 3. Thuật toán sẽ loại bỏ chính xác sau khi sử dụng hết tất cả 16 mặt nạ, xác nhận rằng chỉ tính chẵn lẻ là không đủ và việc kiểm tra tập hợp con đầy đủ là cần thiết ngay cả trong các trường hợp nhỏ.
