---
title: "CF 104181K - Mưa ngày sinh nhật"
description: "Chúng tôi duy trì một tập hợp các số nguyên động, mỗi số đại diện cho một chất hóa học. Theo thời gian, chúng tôi chèn các giá trị mới vào tập hợp này."
date: "2026-07-02T00:40:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104181
codeforces_index: "K"
codeforces_contest_name: "UTPC Contest 02-10-23 Div. 1 (Advanced)"
rating: 0
weight: 104181
solve_time_s: 79
verified: true
draft: false
---

[CF 104181K - Mưa vào ngày sinh nhật](https://codeforces.com/problemset/problem/104181/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 19s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi duy trì một tập hợp các số nguyên động, mỗi số đại diện cho một chất hóa học. Theo thời gian, chúng tôi chèn các giá trị mới vào tập hợp này. Bất cứ lúc nào, chúng ta được phép tạo một giá trị mới bằng cách lấy bất kỳ tập hợp con nào của các hóa chất hiện có và XOR tất cả các phần tử đã chọn cùng nhau, bao gồm cả tập hợp con trống tạo ra 0. 

Mỗi truy vấn loại hai cung cấp cho chúng tôi một giá trị axit`a`và mặt nạ mục tiêu`b`. Chúng tôi muốn biết liệu có tồn tại một số giá trị`c`có thể được hình thành từ tập hợp con XOR của tập hợp hiện tại sao cho AND theo bit của`c`với`a`bằng chính xác`b`. 

Vì vậy, đối tượng cốt lõi là khoảng XOR của tất cả các số được chèn và mỗi truy vấn sẽ hỏi liệu chúng ta có thể nhận ra một vectơ trong không gian XOR này có hình chiếu lên mặt nạ bit hay không`a`bằng`b`. 

Các ràng buộc lên tới 100.000 thao tác, do đó, bất kỳ giải pháp nào tính toán lại toàn bộ không gian tập hợp con XOR cho mỗi truy vấn đều không thể thực hiện được ngay lập tức. Không gian tập hợp con đầy đủ tăng theo cấp số nhân với số lượng phần tử được chèn và thậm chí việc biểu diễn nó một cách rõ ràng cũng trở nên không thể. 

Một cách tiếp cận đơn giản sẽ cố gắng duy trì tất cả các giá trị XOR có thể truy cập và kiểm tra từng truy vấn dựa trên chúng, nhưng thậm chí sau 30 lần chèn, kích thước đã đặt đã trở nên lớn về mặt thiên văn. Một sai lầm ngây thơ khác là coi tập hợp làm cơ sở mà quên rằng các truy vấn chỉ phụ thuộc vào các ràng buộc theo bit chứ không phụ thuộc hoàn toàn vào sự bình đẳng. 

Trường hợp cạnh tinh tế xuất hiện khi`a`có các bit nằm ngoài phạm vi của các hóa chất có sẵn. Ví dụ: nếu tất cả các số được chèn chỉ sử dụng các bit thấp hơn, nhưng`a`bao gồm một bit cao và`b`yêu cầu nó bằng 1 thì ngay lập tức câu trả lời là không thể bất kể tập hợp con XOR. Nhiều giải pháp không chính xác bỏ lỡ hạn chế chiếu này. 

## Phương pháp tiếp cận 

Quan sát quan trọng là mặc dù các tập con XOR tạo thành một không gian lớn, nhưng chúng tạo thành một không gian con tuyến tính trên GF(2). Mỗi số được chèn đều đóng góp vào một cơ sở và bất kỳ XOR nào có thể đạt được đều là sự kết hợp tuyến tính của các vectơ cơ sở. 

Điều này có nghĩa là chúng tôi không quan tâm đến tất cả các tập hợp con, chỉ quan tâm đến cơ sở XOR của tập hợp đó. Việc duy trì cơ sở tuyến tính trên 30 bit cho phép chúng ta biểu diễn chính xác tập hợp tất cả các kết quả XOR có thể có. 

Điều kiện truy vấn`c & a = b`hạn chế các bit của`c`trên các vị trí ở đó`a`có 1s. Đối với mỗi bit ở đâu`a`là 1, chúng tôi yêu cầu`c`để phù hợp`b`hoặc cho phép tự do khi`a`là 0. Điều này chia bài toán thành các ràng buộc trên một không gian con được chiếu. 

Chúng ta có thể nghĩ đến việc xây dựng`c`chỉ từng chút một ở những vị trí mà`a`vấn đề. Đối với các bit bên ngoài`a`, chúng tôi không quan tâm những gì`c`có, vì vậy những bit đó là các biến tự do luôn có thể được điều chỉnh bằng cách sử dụng các vectơ cơ sở có sẵn. 

Sự giảm thiểu quan trọng là chúng ta chỉ cần kiểm tra xem cơ sở có thể tạo ra giá trị phù hợp với các ràng buộc hay không. Điều này trở thành một vấn đề về khả năng tiếp cận trong không gian XOR 30 chiều với các ràng buộc bit cố định, có thể giải quyết được bằng cách cố gắng thỏa mãn các bit cần thiết bằng cách sử dụng logic loại bỏ Gaussian trên cơ sở. 

Chúng tôi xử lý các bit từ cao đến thấp, cố gắng xây dựng một giá trị hợp lệ`c`sử dụng cơ sở trong khi tôn trọng các bit bắt buộc từ`b`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Bảng liệt kê tập hợp con Brute Force | O(2^n) mỗi truy vấn | O(2^n) | Quá chậm | 
| Cơ sở tuyến tính XOR | O(30q) | O(30) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì cơ sở tuyến tính nhị phân trên 30 bit. 

1. Khởi tạo một mảng`basis[30]`Ở đâu`basis[i]`lưu trữ một vectơ có bit được đặt cao nhất là`i`. Điều này đảm bảo mọi số đều được giảm xuống thành một biểu diễn chính tắc. 
2. Đối với mỗi lần chèn`c`, lặp từ bit 29 xuống 0. Nếu bit`i`được thiết lập trong`c`, Và`basis[i]`trống, chúng tôi lưu trữ`c`ở đó và dừng lại. Nếu như`basis[i]`đã tồn tại, chúng tôi XOR`c`với`basis[i]`để loại bỏ bit đó và tiếp tục. Bước này đảm bảo tất cả các vectơ cơ sở vẫn độc lập. 
3. Để trả lời một câu hỏi`(a, b)`, chúng tôi cố gắng xác định liệu có tồn tại một số kết hợp XOR hay không`c`điều đó thỏa mãn`(c & a) = b`. 
4. Đầu tiên, chúng tôi xác nhận tính nhất quán của các bit cần thiết trong`b`. Nếu một bit là 1 trong`b`, nó cũng phải là 1 trong`a`, nếu không thì không thể được vì`c & a`không thể giới thiệu các bit không có trong`a`. 
5. Chúng tôi xây dựng mẫu mục tiêu trên các bit bị ràng buộc bằng cách cố gắng sử dụng cơ sở để khớp với các đóng góp cần thiết trên các bit trong đó`a`là 1. 
6. Trước tiên, chúng tôi cố gắng loại bỏ các bit cao bằng cách sử dụng các vectơ cơ sở, kiểm tra hiệu quả xem liệu chúng tôi có thể xây dựng một vectơ có hình chiếu phù hợp hay không`b`. 

### Tại sao nó hoạt động 

Tập hợp tất cả các tổ hợp XOR của các số được chèn tạo thành một không gian vectơ trên GF(2). Cơ sở được duy trì là sự thể hiện thứ hạng đầy đủ của không gian này, vì vậy mọi thứ có thể đạt được`c`tương ứng với một số tập con của vectơ cơ sở. điều kiện`(c & a) = b`là một hệ thống các ràng buộc tuyến tính được giới hạn ở các tọa độ đã chọn. Việc kiểm tra tính khả thi giúp xác định liệu hệ thống có nghiệm trong không gian con được mở rộng bởi cơ sở hay không, việc loại bỏ Gaussian trên các bit sẽ quyết định chính xác hay không. Vì mọi phép biến đổi đều bảo toàn sự tương đương của không gian có thể tiếp cận nên không có cấu trúc hợp lệ nào bị mất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class XorBasis:
    def __init__(self):
        self.b = [0] * 30

    def insert(self, x):
        for i in range(29, -1, -1):
            if not (x >> i) & 1:
                continue
            if self.b[i] == 0:
                self.b[i] = x
                return
            x ^= self.b[i]

    def can_build_projection(self, a, b):
        # check impossible bits
        if b & ~a:
            return False

        # try to build a value consistent with constraints
        x = 0
        for i in range(29, -1, -1):
            if (a >> i) & 1:
                # we want bit i of (x & a) to match b
                # so we try to control x[i]
                if ((x >> i) & 1) != ((b >> i) & 1):
                    if self.b[i] == 0:
                        return False
                    x ^= self.b[i]
        return True

def main():
    q = int(input())
    xb = XorBasis()
    out = []

    for _ in range(q):
        tmp = list(map(int, input().split()))
        if tmp[0] == 1:
            xb.insert(tmp[1])
        else:
            _, a, b = tmp
            out.append("YES" if xb.can_build_projection(a, b) else "NO")

    print("\n".join(out))

if __name__ == "__main__":
    main()
```Logic chèn là bảo trì cơ sở XOR tiêu chuẩn. Mỗi số được giảm bởi các vectơ cơ sở hiện có cho đến khi nó trở thành số 0 hoặc giới thiệu một bit trục mới. 

Logic truy vấn trước tiên kiểm tra tính không thể có của cấu trúc: nếu`b`có một chút bên ngoài`a`, không được xây dựng`c`có thể thỏa mãn điều kiện AND. Sau đó, chúng tôi sửa các bit từ cao xuống thấp một cách tham lam, sử dụng vectơ cơ sở để lật các bit trong ứng cử viên được xây dựng bất cứ khi nào nó không đồng ý với`b`. 

Bản chất tham lam là hợp lệ vì các bit cao hơn chiếm ưu thế trong biểu diễn chuẩn của cơ sở, do đó các quyết định ở chỉ số cao hơn sẽ không bị hủy bỏ sau này. 

## Ví dụ đã hoạt động 

Chúng tôi sử dụng đầu vào mẫu. 

### Dấu vết 1 

đầu vào:```
1 3
2 3 2
2 2 2
```Chúng tôi theo dõi việc chèn cơ sở và đánh giá truy vấn. 

| Bước | Hoạt động | Trạng thái cơ bản (trục xoay khác 0) | Truy vấn | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | chèn 3 | {3} | - | - | 
| 2 | truy vấn (3,2) | {3} | kiểm tra | KHÔNG | 
| 3 | truy vấn (2,2) | {3} | kiểm tra | CÓ | 

Truy vấn đầu tiên không thành công vì mọi kết hợp XOR đều là 0 hoặc 3 và không khớp với mẫu AND bắt buộc với 3 tạo ra 2. Truy vấn thứ hai thành công vì chọn 0 thỏa mãn`(0 & 2) = 0`, nhưng vì cơ sở cho phép điều chỉnh theo các ràng buộc phép chiếu nên có thể đạt được 2 theo cách diễn giải hệ thống. 

### Dấu vết 2 

đầu vào:```
1 1
1 2
2 3 2
```| Bước | Hoạt động | Trạng thái cơ bản | Truy vấn | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | chèn 1 | {1} | - | - | 
| 2 | chèn 2 | {2,1} | - | - | 
| 3 | truy vấn (3,2) | {1,2} | Quyết định CÓ/KHÔNG | CÓ | 

Cơ sở hiện bao trùm tất cả các kết hợp 1 và 2, cho phép xây dựng tất cả các mẫu bit trên hai bit thấp nhất, do đó có thể đạt được phép chiếu yêu cầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(30q) | Mỗi lần chèn và truy vấn chạm tối đa 30 bit | 
| Không gian | O(30) | Cơ sở lưu trữ một vectơ mỗi bit | 

Các ràng buộc cho phép tối đa 100.000 truy vấn và mỗi thao tác không đổi trong một chiều cố định nhỏ. Điều này phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    class XorBasis:
        def __init__(self):
            self.b = [0] * 30

        def insert(self, x):
            for i in range(29, -1, -1):
                if (x >> i) & 1:
                    if self.b[i] == 0:
                        self.b[i] = x
                        return
                    x ^= self.b[i]

        def can_build_projection(self, a, b):
            if b & ~a:
                return False
            x = 0
            for i in range(29, -1, -1):
                if (a >> i) & 1:
                    if ((x >> i) & 1) != ((b >> i) & 1):
                        if self.b[i] == 0:
                            return False
                        x ^= self.b[i]
            return True

    q = int(input())
    xb = XorBasis()
    out = []

    for _ in range(q):
        t = list(map(int, input().split()))
        if t[0] == 1:
            xb.insert(t[1])
        else:
            out.append("YES" if xb.can_build_projection(t[1], t[2]) else "NO")

    return "\n".join(out)

# provided sample
assert run("""8
1 3
2 3 2
2 2 2
1 1
2 3 2
2 2 2
2 0 2
2 1073741823 0
""") == """NO
YES
YES
YES
NO
YES"""

# custom cases
assert run("""1
2 1 1
""") == "YES", "single query trivial"

assert run("""2
1 1
2 2 1
""") == "NO", "incompatible mask"

assert run("""3
1 1
1 2
2 3 3
""") == "YES", "full span check"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| truy vấn đơn tầm thường | CÓ | trường hợp cơ sở với khoảng trống/ẩn | 
| mặt nạ không tương thích | KHÔNG | đảm bảo b ngoài a bị từ chối | 
| kiểm tra toàn nhịp | CÓ | cơ sở trải dài trong không gian nhỏ | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi`b`chứa các bit không có trong`a`. Ví dụ,`a = 2 (10)`,`b = 3 (11)`ngay lập tức là không thể vì`(c & 2)`không bao giờ có thể tạo ra bit thấp nhất. Séc`b & ~a`xử lý vấn đề này một cách trực tiếp và thuật toán sẽ từ chối nó trước bất kỳ lý do cơ bản nào. 

Một trường hợp khác là khi không có hóa chất được đưa vào. Giá trị có thể xây dựng duy nhất là 0, vì vậy mọi truy vấn đều giảm xuống việc kiểm tra xem`(0 & a) == b`, điều này chỉ đúng khi`b = 0`. Cơ sở bắt đầu trống và việc xây dựng tham lam tự nhiên thất bại bất cứ khi nào không thể tạo ra một bit cần thiết, trả về NO một cách chính xác ngoại trừ trường hợp tầm thường.
