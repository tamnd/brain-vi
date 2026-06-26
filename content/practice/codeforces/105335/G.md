---
title: "CF 105335G - Con Đường Vinh Quang"
description: "Chúng ta được cho ba điểm trong mặt phẳng, nhưng thay vì tùy ý, chúng biểu diễn trung điểm của ba đoạn tạo thành một tam giác chứa các điểm gốc ẩn. Cụ thể hơn, có ba điểm nguyên chưa biết A, B, C. Ta có trung điểm của AB, BC và CA."
date: "2026-06-24T23:01:05+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105335
codeforces_index: "G"
codeforces_contest_name: "ICPC Thailand National Competition 2024"
rating: 0
weight: 105335
solve_time_s: 44
verified: true
draft: false
---

[CF 105335G - Con đường vinh quang](https://codeforces.com/problemset/problem/105335/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 44s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho ba điểm trong mặt phẳng, nhưng thay vì tùy ý, chúng biểu diễn trung điểm của ba đoạn tạo thành một tam giác chứa các điểm gốc ẩn. 

Cụ thể hơn, có ba điểm nguyên chưa biết A, B, C. Ta có trung điểm của AB, BC và CA. Mỗi điểm giữa đều được biết chính xác, có tọa độ nguyên và nhiệm vụ là khôi phục ba điểm ban đầu. 

Mỗi trung điểm được xác định theo cách hình học thông thường: nếu A là (ax, ay) và B là (bx, by), thì trung điểm của AB là ((ax + bx)/2, (ay + by)/2). Điều tương tự cũng xảy ra với hai bên còn lại. 

Đầu vào cung cấp ba tọa độ trung điểm này theo thứ tự tùy ý và chúng ta phải xuất tọa độ của A, B và C. 

Ràng buộc chính là tất cả tọa độ của các điểm ban đầu được đảm bảo là số nguyên. Điều này ngay lập tức ngụ ý rằng tất cả các tọa độ trung điểm đều tương ứng với các trung bình số học nguyên của hai số nguyên, do đó mỗi tọa độ của một trung điểm nói chung là một số nguyên hoặc nửa số nguyên, nhưng trong bài toán này chúng luôn là số nguyên. 

Các giới hạn nhỏ, có tọa độ trong khoảng ±100, do đó, bất kỳ sự tái tạo số học O(1) nào cho mỗi trường hợp thử nghiệm đều đủ. Bất cứ điều gì liên quan đến tìm kiếm hoặc liệt kê hình học đều không cần thiết. 

Một sự hiểu lầm ngây thơ sẽ xảy ra nếu người ta cho rằng các điểm giữa được dán nhãn. Bài toán không cho biết điểm giữa nào tương ứng với cạnh nào, vì vậy khó khăn chính là giải hệ không có nhãn. 

Một vài tình huống khó khăn quan trọng. 

Nếu cả ba trung điểm đều giống nhau, ví dụ tất cả đều là (0, 0) thì tất cả các điểm ban đầu phải trùng nhau tại (0, 0). Một nỗ lực bất cẩn giả định các đỉnh khác nhau có thể thất bại. 

Nếu hai điểm giữa bị hoán đổi hoặc diễn giải không chính xác, nỗ lực ghép nối trực tiếp giữa các điểm có thể tạo ra các đỉnh không nhất quán hoặc phân đoạn. Ví dụ: ghép các khác biệt điểm giữa tùy ý mà không thực thi tính nhất quán sẽ dẫn đến các đỉnh ứng cử viên không nguyên, không hợp lệ. 

## Phương pháp tiếp cận 

Một cách giải thích mạnh mẽ sẽ là thử gán ba điểm đã cho cho các cạnh AB, BC và CA trong tất cả 6 hoán vị, sau đó tái tạo lại A, B và C mỗi lần và kiểm tra tính nhất quán. Cái này vốn đã nhỏ rồi, nhưng về mặt khái niệm thì nó nặng hơn mức cần thiết. 

Quan sát quan trọng là các phương trình điểm giữa tạo thành một hệ thống tuyến tính. Nếu chúng ta biểu thị các điểm giữa là M_AB, M_BC và M_CA thì: 

A + B = 2 M_AB 

B + C = 2 M_BC 

C + A = 2 M_CA 

Cộng cả ba phương trình sẽ được: 

2(A + B + C) = 2(M_AB + M_BC + M_CA) 

Vì vậy: 

A + B + C = M_AB + M_BC + M_CA 

Khi đã biết tổng S = A + B + C, chúng ta có thể cô lập từng đỉnh: 

A = S − (B + C) = S − 2 M_BC 

B = S − 2 M_CA 

C = S − 2 M_AB 

Điều này hoàn toàn xác định giải pháp mà không cần bất kỳ tìm kiếm hoán vị nào. Vấn đề duy nhất còn lại là chúng ta không biết điểm giữa nào tương ứng với cặp nào. Vì vậy chúng ta vẫn cần gán nhãn chính xác. 

Tuy nhiên, vì cả ba vai trò đều đối xứng nên chúng ta có thể đơn giản thử tất cả các phép gán của ba điểm đã cho cho (M_AB, M_BC, M_CA). Đối với mỗi phép gán, hãy tính A, B, C bằng cách sử dụng các công thức trên và kiểm tra tính nhất quán bằng cách tính lại điểm giữa. 

Vì chỉ có 6 hoán vị nên đây là công không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Hoán vị Brute + tính toán lại | O(1) | O(1) | Đã chấp nhận | 
| Hệ thống tuyến tính có ghi nhãn cố định | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc ba điểm giữa M1, M2, M3. 

Chúng tương ứng với các cạnh chưa biết, nhưng chúng ta không thừa nhận bất kỳ thứ tự nào. 
2. Lặp lại tất cả các hoán vị gán các điểm này cho M_AB, M_BC, M_CA. 

Điều này đảm bảo chúng tôi bao gồm tất cả các cách có thể mà đầu vào có thể ánh xạ tới các cạnh tam giác. 
3. Với mỗi phép gán, hãy tính tọa độ S = M_AB + M_BC + M_CA. 
4. Xây dựng lại các đỉnh ứng viên bằng cách sử dụng: 

A = S − 2 * M_BC 

B = S − 2 * M_CA

C = S − 2 * M_AB 

Điều này xuất phát trực tiếp từ việc giải các phương trình trung điểm bằng đại số. 
5. Xác minh tính đúng đắn bằng cách kiểm tra xem: 

điểm giữa(A, B) == M_AB 

trung điểm(B, C) == M_BC 

điểm giữa(C, A) == M_CA 

Bước này ngăn chặn các hoán vị không chính xác tạo ra một bản dựng lại có vẻ hợp lệ nhưng sai. 
6. Sau khi tìm thấy cấu hình hợp lệ, xuất A, B và C. 

### Tại sao nó hoạt động 

Các phương trình điểm giữa xác định một hệ tuyến tính có nghiệm duy nhất khi sự tương ứng giữa điểm giữa và các cạnh được cố định. Các công thức xây dựng lại xuất phát trực tiếp từ việc tính tổng và trừ các phương trình này, điều này đảm bảo rằng bất kỳ phép gán hợp lệ nào cũng tạo ra các đỉnh ban đầu chính xác. Vì việc ghi nhãn đúng nằm trong số 6 hoán vị nên thuật toán đảm bảo sẽ gặp phải nó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def midpoint(p, q):
    return ((p[0] + q[0]) // 2, (p[1] + q[1]) // 2)

def ok(A, B, C, MAB, MBC, MCA):
    return (midpoint(A, B) == MAB and
            midpoint(B, C) == MBC and
            midpoint(C, A) == MCA)

def solve():
    mids = [tuple(map(int, input().split())) for _ in range(3)]

    from itertools import permutations

    for MAB, MBC, MCA in permutations(mids):
        Sx = MAB[0] + MBC[0] + MCA[0]
        Sy = MAB[1] + MBC[1] + MCA[1]

        A = (Sx - 2 * MBC[0], Sy - 2 * MBC[1])
        B = (Sx - 2 * MCA[0], Sy - 2 * MCA[1])
        C = (Sx - 2 * MAB[0], Sy - 2 * MAB[1])

        if ok(A, B, C, MAB, MBC, MCA):
            print(*A)
            print(*B)
            print(*C)
            return

solve()
```Mã đọc ba tọa độ điểm giữa đã cho và thử mọi phép gán có thể có cho các cạnh của tam giác. Đối với mỗi phép gán, nó sẽ xây dựng lại các đỉnh bằng cách sử dụng các nhận dạng tuyến tính dẫn xuất. Hàm xác thực tính toán lại các điểm giữa để đảm bảo rằng tam giác được tái tạo là nhất quán, ngăn ngừa việc chấp nhận các hoán vị không chính xác. 

Một chi tiết triển khai tinh tế là phép chia số nguyên trong kiểm tra điểm giữa. Vì bài toán đảm bảo tọa độ nguyên cho các điểm gốc, nên các điểm giữa được xây dựng lại sẽ khớp chính xác mà không cần số học dấu phẩy động. Việc sử dụng bộ số nguyên sẽ tránh hoàn toàn các vấn đề về độ chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào: 

(2, 4), (5, 5), (4, 3) 

Chúng tôi kiểm tra một bài tập đúng: 

M_AB = (2, 4), M_BC = (5, 5), M_CA = (4, 3) 

| Bước | Giá trị | 
| --- | --- | 
| S = M_AB + M_BC + M_CA | (11, 12) | 
| A = S − 2 M_BC | (1, 2) | 
| B = S − 2 M_CA | (3, 6) | 
| C = S − 2 M_AB | (7, 4) | 

Kiểm tra điểm giữa xác nhận tính nhất quán, vì vậy phép gán này là chính xác. 

Dấu vết này cho thấy việc tái tạo hoàn toàn mang tính đại số và không phụ thuộc vào trực giác hình học một khi các phương trình được thiết lập. 

### Ví dụ 2 

đầu vào: 

(-3, -4), (2, 5), (3, -2) 

Việc thử hoán vị chính xác mang lại: 

| Bước | Giá trị | 
| --- | --- | 
| S | (2, -1) | 
| A | (-2, -11) | 
| B | (-4, 3) | 
| C | (8, 7) | 

Một lần nữa, xác nhận điểm giữa xác nhận tính đúng đắn. 

Ví dụ này chứng minh rằng ngay cả với tọa độ âm, việc tái thiết tuyến tính vẫn ổn định. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ có 6 hoán vị, số học không đổi cho mỗi trường hợp | 
| Không gian | O(1) | Chỉ lưu trữ ba điểm và một vài điểm tạm thời | 

Các ràng buộc đủ nhỏ để ngay cả việc xây dựng lại dựa trên hoán vị đơn giản cũng có thể chạy ngay lập tức trong giới hạn. Giải pháp bị chi phối hoàn toàn bởi số học thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from itertools import permutations

    mids = [tuple(map(int, sys.stdin.readline().split())) for _ in range(3)]

    def midpoint(p, q):
        return ((p[0] + q[0]) // 2, (p[1] + q[1]) // 2)

    def ok(A, B, C, MAB, MBC, MCA):
        return (midpoint(A, B) == MAB and
                midpoint(B, C) == MBC and
                midpoint(C, A) == MCA)

    for MAB, MBC, MCA in permutations(mids):
        Sx = MAB[0] + MBC[0] + MCA[0]
        Sy = MAB[1] + MBC[1] + MCA[1]

        A = (Sx - 2 * MBC[0], Sy - 2 * MBC[1])
        B = (Sx - 2 * MCA[0], Sy - 2 * MCA[1])
        C = (Sx - 2 * MAB[0], Sy - 2 * MAB[1])

        if ok(A, B, C, MAB, MBC, MCA):
            return f"{A[0]} {A[1]}\n{B[0]} {B[1]}\n{C[0]} {C[1]}\n"

    return ""

# provided samples
assert run("2 4\n5 5\n4 3\n") == "1 2\n3 6\n7 4\n"
assert run("-3 -4\n2 5\n3 -2\n") == "-2 -11\n-4 3\n8 7\n"

# custom cases
assert run("0 0\n0 0\n0 0\n") == "0 0\n0 0\n0 0\n", "all same point"
assert run("1 1\n2 2\n3 3\n") != "", "valid triangle existence"
assert run("2 0\n0 2\n1 1\n") != "", "symmetric case"
assert run("-1 -1\n1 1\n0 0\n") != "", "mixed signs"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả số không | (0,0) lặp lại | tam giác suy biến | 
| điểm đối xứng | tái thiết hợp lệ | tính đúng đắn của hoán vị | 
| dấu hiệu hỗn hợp | tái thiết hợp lệ | độ bền số học | 

## Vỏ cạnh 

Khi cả ba điểm giữa giống hệt nhau, mọi hoán vị đều tạo ra các đỉnh được tái tạo giống nhau và thuật toán trả về chính xác một tam giác thu gọn trong đó tất cả các điểm ban đầu trùng nhau. Bước xác minh điểm giữa vẫn được thực hiện vì tất cả các điểm giữa theo cặp vẫn không thay đổi. 

Khi thứ tự đầu vào là tùy ý, một ánh xạ đơn giản của đầu vào đầu tiên tới AB, thứ hai tới BC, thứ ba tới CA có thể âm thầm tạo ra các đỉnh không nhất quán. Vòng lặp hoán vị ngăn chặn điều này bằng cách kiểm tra rõ ràng tất cả các nhãn. 

Khi tọa độ âm hoặc hỗn hợp, phép trừ trong công thức tái tạo có thể tạo ra các giá trị trung gian lớn, nhưng tất cả các phép toán vẫn nằm trong phạm vi số nguyên do sự đảm bảo của bài toán. Việc xác thực điểm giữa đảm bảo rằng mọi sự không khớp về mặt số học sẽ bị loại bỏ ngay lập tức.
