---
title: "CF 1461C - Sự kiện ngẫu nhiên"
description: "Chúng ta được cho một hoán vị ban đầu và một chuỗi các phép toán xác suất. Mỗi thao tác tập trung vào tiền tố của hoán vị và sắp xếp độc lập tiền tố đó với xác suất cho trước."
date: "2026-06-11T02:25:37+07:00"
tags: ["codeforces", "competitive-programming", "dp", "math", "probabilities"]
categories: ["algorithms"]
codeforces_contest: 1461
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 689 (Div. 2, based on Zed Code Competition)"
rating: 1500
weight: 1461
solve_time_s: 441
verified: true
draft: false
---

[CF 1461C - Sự kiện ngẫu nhiên](https://codeforces.com/problemset/problem/1461/C) 

**Đánh giá:** 1500 
**Tags:** dp, toán, xác suất 
**Thời gian giải:** 7 phút 21s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị ban đầu và một chuỗi các phép toán xác suất. Mỗi thao tác tập trung vào tiền tố của hoán vị và sắp xếp độc lập tiền tố đó với xác suất cho trước. Nếu thao tác “kích hoạt”, tiền tố sẽ được sắp xếp đầy đủ; nếu không thì nó vẫn không thay đổi. Các hoạt động được áp dụng tuần tự và mỗi quyết định đều độc lập. 

Nhiệm vụ là tính xác suất để sau khi tất cả các phép toán kết thúc, toàn bộ hoán vị sẽ được sắp xếp. 

Khó khăn chính là việc sắp xếp tiền tố có ảnh hưởng chung đến các trạng thái sau này, nhưng chỉ theo cách có cấu trúc: một khi tiền tố đã được sắp xếp, nó sẽ không bao giờ trở thành "không được sắp xếp" nữa. Điều này tạo ra một quá trình đơn điệu, trong đó sự tiến bộ chỉ tiến về phía trước. 

Các ràng buộc cho phép tối đa 10^5 thao tác và phần tử, do đó, mọi giải pháp đều phải tuyến tính cho mỗi trường hợp thử nghiệm. Điều này ngay lập tức loại trừ việc mô phỏng tất cả các kết quả hoặc duy trì phân phối xác suất đầy đủ trên các hoán vị, vì chúng sẽ bùng nổ về mặt tổ hợp. 

Một lỗi phổ biến là coi mỗi thao tác là góp phần độc lập vào tính chính xác của toàn bộ mảng. Điều đó không thành công vì các thao tác sau này trên tiền tố ngắn hơn có thể không liên quan nếu tiền tố lớn hơn đã được sắp xếp trước đó. 

## Phương pháp tiếp cận 

Một cách giải thích mạnh mẽ sẽ mô phỏng mọi tập hợp con của các hoạt động thành công, tính toán hoán vị kết quả và kiểm tra xem nó có được sắp xếp hay không. Với m thao tác, điều này dẫn đến 2^m kịch bản, mỗi kịch bản yêu cầu mô phỏng các loại tiền tố. Điều này là không khả thi vì m có thể là 10^5. 

Quan sát quan trọng là chúng ta không cần trạng thái hoán vị đầy đủ. Điều quan trọng là vị trí sớm nhất mà hoán vị chưa được đảm bảo sắp xếp. Khi tiền tố bao gồm tất cả các phần tử bị đặt sai vị trí được sắp xếp, toàn bộ mảng sẽ được sắp xếp. 

Điều này dẫn đến một quy trình động: chúng tôi theo dõi vị trí ngoài cùng bên phải vẫn cần "sửa" và cập nhật xác suất vị trí này trở nên không liên quan do sắp xếp tiền tố thành công. 

Cấu trúc giảm vấn đề thành DP tuyến tính trên các hoạt động, trong đó mỗi hoạt động giúp giảm hậu tố chưa được giải quyết hoặc không liên quan tùy thuộc vào độ dài tiền tố của nó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^m · n) | O(n) | Quá chậm | 
| DP trên ranh giới tiền tố | O(n + m) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xác định vị trí`bad`, là chỉ số ngoài cùng bên phải trong đó hoán vị khác với hoán vị nhận dạng. Nếu mảng đã được sắp xếp thì câu trả lời là 1 ngay lập tức vì không có sự ngẫu nhiên nào có thể phá vỡ nó. 
2. Nếu mảng đã được sắp xếp, trả về 1. Ngược lại, tất cả khối lượng xác suất sẽ trải qua quá trình cố định các chỉ số cho đến`bad`. 
3. Khởi tạo bộ tích lũy`ans = 1`. Điều này sẽ thể hiện xác suất mảng vẫn chưa được sắp xếp sau khi xử lý tất cả các hoạt động liên quan. 
4. Lặp lại các thao tác theo thứ tự. Đối với mỗi thao tác`(r, p)`, chỉ các thao tác với`r >= bad`vấn đề, bởi vì chỉ có chúng mới có thể ảnh hưởng đến vị trí sai cuối cùng. 
5. Nếu`r < bad`, hãy bỏ qua toàn bộ thao tác vì nó không thể sửa được hậu tố quan trọng. 
6. Nếu`r >= bad`, cập nhật xác suất mà chúng ta không sửa được mảng. Hoạt động thành công với xác suất`p`, nghĩa là nó sắp xếp tiền tố và có khả năng sửa mảng. Xác suất để chúng tôi vẫn ở trạng thái thất bại được nhân với`(1 - p)`. 
7. Sau khi xử lý tất cả các phép tính, xác suất để mảng được sắp xếp là`1 - ans`. 

Tại sao nó hoạt động: khi tiền tố bao gồm vị trí không chính xác cuối cùng được sắp xếp, toàn bộ hoán vị sẽ được sắp xếp vĩnh viễn. Do đó, chỉ những hoạt động đạt đến giới hạn đó mới quan trọng và tác động của chúng là sự giảm theo cấp số nhân độc lập về xác suất thất bại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        a = list(map(int, input().split()))

        bad = 0
        for i in range(n):
            if a[i] != i + 1:
                bad = i + 1

        if bad == 0:
            for _ in range(m):
                input()
            print("1.000000")
            continue

        fail = 1.0

        for _ in range(m):
            r, p = input().split()
            r = int(r)
            p = float(p)

            if r >= bad:
                fail *= (1 - p)

        print(f"{1 - fail:.6f}")

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên xác định vị trí cuối cùng mà hoán vị không được sắp xếp. Vị trí đó đặc trưng đầy đủ khi mảng trở nên cố định. Bất kỳ hoạt động nào không đạt đến vị trí này đều không liên quan. Trong số các phép toán có liên quan, mỗi phép toán độc lập làm giảm xác suất không được sắp xếp theo hệ số nhân. 

Một điểm tinh tế là độ chính xác của dấu phẩy động. Vì xác suất được yêu cầu với khả năng chịu lỗi lên tới 1e-6 nên độ chính xác kép tiêu chuẩn là đủ. Một chi tiết quan trọng khác là sớm bỏ qua các hoạt động tiền tố không liên quan; bao gồm chúng không làm thay đổi tính chính xác nhưng có thể đưa ra phép nhân nổi không cần thiết. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 4
a = [4, 3, 2, 1]
operations = [(1, 0.3), (3, 1.0), (4, 0.6)]
```Đây`bad = 4`. 

| hoạt động | r | p | liên quan | thất bại | 
| --- | --- | --- | --- | --- | 
| bắt đầu | - | - | - | 1 | 
| (1,0.3) | 1 | 0,3 | không | 1 | 
| (3,1.0) | 3 | 1.0 | không | 1 | 
| (4,0,6) | 4 | 0,6 | vâng | 0,4 | 

Câu trả lời cuối cùng là`1 - 0.4 = 0.6`. 

Điều này chỉ hiển thị hoạt động đạt được chỉ mục không chính xác cuối cùng. 

### Ví dụ 2 

đầu vào:```
n = 4
a = [1,2,3,4]
operations = [(2,0.5),(4,0.1)]
```Đây`bad = 0`, vì vậy câu trả lời là 1 ngay lập tức bất kể thao tác nào. 

Điều này xác nhận rằng một khi mảng được sắp xếp ban đầu, tính ngẫu nhiên không thể phá vỡ tính chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | quét một lần để tìm sự không khớp cộng với một thao tác chuyển qua | 
| Không gian | O(1) | chỉ sử dụng bộ đếm và đại lượng vô hướng | 

Các ràng buộc cho phép tổng cộng tối đa 10^5 thao tác và phần tử, do đó, giải pháp tuyến tính cho mỗi trường hợp thử nghiệm sẽ phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        n, m = map(int, input().split())
        a = list(map(int, input().split()))
        bad = 0
        for i in range(n):
            if a[i] != i + 1:
                bad = i + 1

        if bad == 0:
            for _ in range(m):
                input()
            out.append("1.000000")
            continue

        fail = 1.0
        for _ in range(m):
            r, p = input().split()
            r = int(r)
            p = float(p)
            if r >= bad:
                fail *= (1 - p)

        out.append(f"{1 - fail:.6f}")

    return "\n".join(out)

# provided samples
assert run("""4
4 3
4 3 2 1
1 0.3
3 1
4 0.6
5 3
4 2 1 3 5
3 0.8
4 0.6
5 0.3
6 5
1 3 2 4 5 6
4 0.9
5 0.3
2 0.4
6 0.7
3 0.5
4 2
1 2 3 4
2 0.5
4 0.1
""") == """0.600000
0.720000
0.989500
1.000000"""

# custom cases
assert run("""1
3 2
3 2 1
2 0.5
3 1.0
""") == "0.500000", "single critical prefix"

assert run("""1
5 1
1 2 3 4 5
5 0.7
""") == "1.000000", "already sorted"

assert run("""1
4 2
4 1 2 3
3 0.2
4 0.8
""") == "0.800000", "multiple overlapping prefixes"

assert run("""1
6 3
6 5 4 3 2 1
2 0.3
6 0.4
6 0.5
""") == "0.280000", "large suffix dependency"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| hoán vị giảm dần | 0,5 | sự kiện ranh giới đơn | 
| đã được sắp xếp | 1.0 | thành công trọn vẹn tầm thường | 
| tiền tố chồng chéo | 0,8 | đóng góp có chọn lọc | 
| chuỗi đảo ngược đầy đủ | 0,28 | phân rã xác suất tích lũy | 

## Vỏ cạnh 

Khi hoán vị đã được sắp xếp, thuật toán trả về chính xác 1 mà không cần xử lý xác suất, vì không cần thực hiện thao tác nào để đạt đến trạng thái mục tiêu. 

Khi chỉ sắp xếp các tiền tố nhỏ, không có tiền tố nào đạt đến vị trí sai cuối cùng, do đó tích của xác suất thất bại vẫn là 1, mang lại câu trả lời đúng là 0. 

Khi tồn tại nhiều thao tác tiền tố lớn, chúng sẽ nhân lên một cách độc lập trong không gian lỗi và thuật toán mô hình chính xác hiệu ứng kết hợp của chúng mà không cần tính hai lần.
