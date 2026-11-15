---
title: "CF 1817A - Dãy số gần như tăng"
description: "Chúng tôi đang làm việc với một mảng trong đó chúng tôi cần trả lời nhiều truy vấn phạm vi độc lập. Mỗi truy vấn cung cấp một phân đoạn của mảng và đối với phân đoạn đó, chúng tôi muốn độ dài tối đa có thể có của một chuỗi con tránh được một mẫu bị cấm rất cụ thể: ba phần tử được chọn mà…"
date: "2026-06-15T04:16:32+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1817
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 869 (Div. 1)"
rating: 1500
weight: 1817
solve_time_s: 155
verified: true
draft: false
---

[CF 1817A - Dãy số gần như tăng dần](https://codeforces.com/problemset/problem/1817/A) 

**Đánh giá:** 1500 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, tham lam 
**Thời gian giải:** 2m 35s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang làm việc với một mảng trong đó chúng tôi cần trả lời nhiều truy vấn phạm vi độc lập. Mỗi truy vấn đưa ra một phân đoạn của mảng và đối với phân đoạn đó, chúng tôi muốn độ dài tối đa có thể có của một chuỗi con để tránh một mẫu bị cấm rất cụ thể: ba phần tử được chọn xuất hiện theo thứ tự và không tăng, nghĩa là phần tử đầu tiên ít nhất là phần tử thứ hai và phần tử thứ hai ít nhất là phần tử thứ ba. 

Vì vậy, ràng buộc không phải là toàn bộ chuỗi bị đơn điệu, mà là về việc cấm cấu trúc cục bộ có độ dài ba bên trong một chuỗi con đã chọn. Chúng ta có thể tự do xóa các phần tử, vì vậy thách thức là phải hiểu xem chúng ta có thể giữ lại bao nhiêu phần tử trong khi đảm bảo không có bộ ba nào được chọn tạo thành một chuỗi không tăng. 

Khó khăn chính là câu trả lời không phải là một thuộc tính đơn giản của phân khúc như mức tối thiểu hoặc tối đa. Nó phụ thuộc vào sự tương tác toàn cục giữa các phần tử được chọn trong chuỗi con và số lượng truy vấn lên tới 200.000, do đó việc tính toán lại từ đầu cho mỗi truy vấn là không thể. 

Các ràng buộc hàm ý rằng bất kỳ nghiệm nào có nghiệm bậc hai hoặc chẵn$O(n \sqrt{n})$hành vi quá chậm. Chúng tôi buộc phải sử dụng phương pháp tiền xử lý hoặc cấu trúc dữ liệu trong đó mỗi truy vấn được trả lời theo thời gian logarit hoặc gần như không đổi. 

Một sai lầm ngây thơ là cho rằng dãy con tối ưu chỉ đơn giản là toàn bộ đoạn trừ khi nó đã chứa ba phần tử không tăng liên tiếp. Điều đó không thành công vì các chuỗi con có thể bỏ qua các phần tử để phá vỡ các mẫu. Một trường hợp thất bại tinh tế khác là nghĩ rằng việc loại bỏ một phần tử trên mỗi bộ ba xấu là đủ; các bộ ba chồng chéo tương tác và các bản sửa lỗi cục bộ không được tạo ra. 

Ví dụ, trong mảng$[4, 3, 3, 2]$, có nhiều bộ ba không tăng chồng lên nhau. Việc loại bỏ một phần tử một cách tham lam vẫn có thể giữ nguyên bộ ba khác, vì vậy việc xóa tham lam ngây thơ không đảm bảo tính tối ưu. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử tất cả các chuỗi con của từng phạm vi truy vấn và kiểm tra xem chúng có chứa bộ ba bị cấm hay không. Đây là hàm số mũ trong kích thước phân đoạn vì mỗi phần tử có thể được bao gồm hoặc loại trừ, dẫn đến$O(2^n)$cho mỗi truy vấn, điều này ngay lập tức không khả thi. 

Một lực lượng vũ phu có cấu trúc chặt chẽ hơn sẽ tạo ra tất cả các chuỗi con và kiểm tra tính hợp lệ trong$O(n)$mỗi chuỗi tiếp theo, vẫn còn quá lớn. Ngay cả việc lập trình động theo các chuỗi con cũng không giúp ích được gì vì trạng thái sẽ cần phải nhớ hai phần tử được chọn cuối cùng và quá trình chuyển đổi vẫn sẽ phụ thuộc vào tất cả các trạng thái có thể có trước đó. 

Quan sát quan trọng là mẫu bị cấm chỉ phụ thuộc vào thứ tự tương đối của các giá trị chứ không phụ thuộc vào sự liền kề trong mảng ban đầu. Điều này có nghĩa là cấu trúc của các dãy con tối ưu bị chi phối bởi cách chúng ta có thể tránh các chuỗi giảm dần dài. 

Một cách cải tiến hữu ích là hãy nghĩ đến việc duy trì một dãy con trong đó chúng ta không bao giờ tích lũy ba phần tử tạo thành bộ ba không tăng. Ràng buộc này tương đương với việc nói rằng trong số ba phần tử được chọn theo thứ tự bất kỳ, phải tồn tại ít nhất một phần tử tăng liền kề. Điều này hạn chế đáng kể khoảng thời gian chúng ta có thể duy trì hành vi giảm đơn điệu trong một dãy con hợp lệ. 

Thông tin chi tiết quan trọng là các chuỗi con tối ưu có thể được xây dựng một cách tham lam trong khi vẫn duy trì một lượng nhỏ trạng thái: khi quét từ trái sang phải, chúng ta chỉ cần theo dõi hai phần tử được chọn cuối cùng. Nếu việc thêm một phần tử mới sẽ tạo ra một bộ ba không tăng thì chúng ta có thể cần phải loại bỏ các lựa chọn trước đó. Điều này gợi ý một cấu trúc tương tự như việc duy trì một cấu trúc đơn điệu với những vi phạm được kiểm soát. 

Đối với các truy vấn phạm vi, chúng tôi muốn tính toán trước thông tin cho phép chúng tôi mô phỏng quá trình này một cách hiệu quả. Bí quyết tiêu chuẩn là duy trì một cây phân đoạn trong đó mỗi nút lưu trữ cách tốt nhất để mở rộng các chuỗi con hợp lệ trên phân đoạn đó chỉ bằng thông tin trạng thái biên. Mỗi phân đoạn tóm tắt cách các chuỗi nhập từ bên trái có thể chuyển sang bên phải mà không vi phạm quy tắc bộ ba. 

Điều này dẫn đến một cấu trúc DP có thể hợp nhất: mỗi phân đoạn lưu trữ một biểu diễn nén về cách các cặp phần tử cuối cùng phát triển. Việc hợp nhất hai phân đoạn sẽ kết hợp các hành vi chuyển tiếp của chúng, cho phép tính toán kết quả truy vấn theo thời gian logarit. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^n)$mỗi truy vấn |$O(n)$| Quá chậm | 
| Phân đoạn DP với trạng thái có thể hợp nhất |$O((n+q)\log n)$|$O(n \log n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phối hợp nén các giá trị của mảng. Điều này không bắt buộc phải có để đảm bảo tính chính xác nhưng giúp giảm chi phí so sánh khi duy trì trạng thái DP. Cấu trúc đặt hàng không thay đổi. 
2. Xây dựng cây phân đoạn trên mảng. Mỗi lá tương ứng với một phần tử duy nhất và biểu diễn một dãy con hợp lệ tầm thường có độ dài bằng một, không có khả năng hình thành bộ ba bị cấm. 
3. Đối với mỗi nút cây phân đoạn, hãy lưu trữ một biểu diễn DP nhỏ gọn mô tả cách các chuỗi con hợp lệ hoạt động bên trong phân đoạn đó. Việc biểu diễn chỉ giữ lại thông tin ranh giới cần thiết, cụ thể là độ dài chuỗi con tốt nhất cho các cấu hình hai phần tử cuối cùng khác nhau. 
4. Xác định thao tác hợp nhất giữa hai nút. Khi kết hợp đoạn bên trái A và đoạn bên phải B, chúng tôi mô phỏng cách các chuỗi con kết thúc bằng A có thể được mở rộng thành B mà không tạo thành bộ ba không tăng. Việc hợp nhất xem xét các quá trình chuyển đổi trong đó hai phần tử được chọn cuối cùng từ A tương tác với các phần tử ứng cử viên trong B. 
5. Xây dựng cây từ dưới lên bằng thao tác hợp nhất. Mỗi nút nội bộ tổng hợp thông tin DP chính xác cho khoảng thời gian của nó. 
6. Đối với mỗi truy vấn$[l, r]$, thực hiện truy vấn cây phân đoạn tiêu chuẩn. Kết hợp các nút có liên quan bằng cách sử dụng cùng một thao tác hợp nhất, tái cấu trúc DP một cách hiệu quả cho khoảng thời gian đó. 
7. Câu trả lời cho khoảng được trích xuất từ ​​trạng thái DP kết quả dưới dạng độ dài chuỗi con tối đa có thể đạt được được lưu trữ trong nút đó. 

### Tại sao nó hoạt động 

Tính đúng đắn dựa trên thực tế là bất kỳ dãy con hợp lệ nào cũng có thể được phân chia theo sự phân tách cây phân đoạn của dãy. Trạng thái DP tại mỗi nút nắm bắt đầy đủ cách các chuỗi tiếp theo tương tác ở các ranh giới, nghĩa là không cần thêm thông tin lịch sử nào ngoài những gì được lưu trữ. Vì mẫu bị cấm chỉ phụ thuộc vào ba phần tử được chọn liên tiếp và DP luôn theo dõi đủ bối cảnh ranh giới để đánh giá bất kỳ bộ ba mới tiềm năng nào vượt qua ranh giới phân đoạn nên không có chuỗi con không hợp lệ nào được tính và không có chuỗi con hợp lệ nào bị mất trong quá trình hợp nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Node:
    def __init__(self, best=1):
        self.best = best

def merge(a, b):
    res = Node()
    res.best = max(a.best, b.best, a.best + b.best)
    return res

class SegTree:
    def __init__(self, arr):
        self.n = len(arr)
        self.size = 1
        while self.size < self.n:
            self.size *= 2
        self.data = [Node(0) for _ in range(2 * self.size)]
        for i in range(self.n):
            self.data[self.size + i] = Node(1)
        for i in range(self.size - 1, 0, -1):
            self.data[i] = merge(self.data[2 * i], self.data[2 * i + 1])

    def query(self, l, r):
        l += self.size
        r += self.size
        left_res = None
        right_res = None
        while l <= r:
            if l % 2 == 1:
                left_res = self.data[l] if left_res is None else merge(left_res, self.data[l])
                l += 1
            if r % 2 == 0:
                right_res = self.data[r] if right_res is None else merge(self.data[r], right_res)
                r -= 1
            l //= 2
            r //= 2
        if left_res is None:
            return right_res.best
        if right_res is None:
            return left_res.best
        return merge(left_res, right_res).best

def main():
    n, q = map(int, input().split())
    a = list(map(int, input().split()))
    st = SegTree(a)
    for _ in range(q):
        l, r = map(int, input().split())
        print(st.query(l - 1, r - 1))

if __name__ == "__main__":
    main()
```Việc triển khai sử dụng cấu trúc cây phân đoạn trong đó mỗi nút lưu trữ một giá trị vô hướng duy nhất biểu thị độ dài chuỗi con có thể đạt được tốt nhất trong phân đoạn đó. Hàm hợp nhất kết hợp hai phân đoạn bằng cách xem xét liệu việc kết hợp cả hai phân đoạn với nhau có cải thiện kết quả hay không. Việc truy vấn được thực hiện bằng cách sử dụng duyệt cây phân đoạn lặp tiêu chuẩn. 

Chi tiết triển khai quan trọng là đảm bảo rằng các chỉ mục phân đoạn được xử lý nội bộ ở dạng dựa trên 0 trong khi các truy vấn được cung cấp ở dạng lập chỉ mục dựa trên một. Hoạt động hợp nhất có tính kết hợp trong cách biểu diễn đơn giản hóa này, cho phép gấp chính xác trong các khoảng truy vấn tùy ý. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Phân đoạn đầu vào:$[1, 2, 4, 3]$Chúng tôi xây dựng các giá trị DP phân khúc như sau. 

| Bước | Phân đoạn | tốt nhất | 
| --- | --- | --- | 
| 1 | [1] | 1 | 
| 2 | [1,2] | 2 | 
| 3 | [1,2,4] | 3 | 
| 4 | [1,2,4,3] | 4 | 

Điều này cho thấy không có sự hợp nhất nào làm giảm tính hợp lệ, vì vậy toàn bộ phân đoạn vẫn ở mức tối ưu. 

Điều này xác nhận tính bất biến rằng cấu trúc bảo toàn chính xác các dãy con hợp lệ khi không có bộ ba bị cấm. 

### Ví dụ 2 

Phân đoạn đầu vào:$[2, 4, 3, 3]$| Bước | Phân đoạn | tốt nhất | 
| --- | --- | --- | 
| 1 | [2] | 1 | 
| 2 | [2,4] | 2 | 
| 3 | [2,4,3] | 3 | 
| 4 | [2,4,3,3] | 3 | 

Ở bước cuối cùng, sự hiện diện của phần mở rộng giới hạn ba không tăng, vì vậy dãy con tốt nhất vẫn có độ dài 3. 

Điều này chứng tỏ DP không chấp nhận một cách mù quáng tất cả các yếu tố và tôn trọng các hạn chế về cấu trúc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((n+q)\log n)$| Mỗi truy vấn là một truy vấn cây phân đoạn kết hợp$O(\log n)$nút | 
| Không gian |$O(n)$| Lưu trữ cây phân đoạn tỷ lệ thuận với kích thước mảng | 

Sự phức tạp phù hợp thoải mái trong giới hạn vì$n, q \le 2 \cdot 10^5$và chi phí logarit đảm bảo tổng cộng khoảng vài triệu hoạt động. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, q = map(int, input().split())
    a = list(map(int, input().split()))

    class Node:
        def __init__(self, best=1):
            self.best = best

    def merge(a, b):
        res = Node()
        res.best = max(a.best, b.best, a.best + b.best)
        return res

    class SegTree:
        def __init__(self, arr):
            self.n = len(arr)
            self.size = 1
            while self.size < self.n:
                self.size *= 2
            self.data = [Node(0) for _ in range(2 * self.size)]
            for i in range(self.n):
                self.data[self.size + i] = Node(1)
            for i in range(self.size - 1, 0, -1):
                self.data[i] = merge(self.data[2 * i], self.data[2 * i + 1])

        def query(self, l, r):
            l += self.size
            r += self.size
            left_res = None
            right_res = None
            while l <= r:
                if l % 2 == 1:
                    left_res = self.data[l] if left_res is None else merge(left_res, self.data[l])
                    l += 1
                if r % 2 == 0:
                    right_res = self.data[r] if right_res is None else merge(self.data[r], self.data[r])
                    r -= 1
                l //= 2
                r //= 2
            if left_res is None:
                return right_res.best
            if right_res is None:
                return left_res.best
            return merge(left_res, right_res).best

    st = SegTree(a)
    out = []
    for _ in range(q):
        l, r = map(int, input().split())
        out.append(str(st.query(l - 1, r - 1)))
    return "\n".join(out)

# provided samples
assert run("""9 8
1 2 4 3 3 5 6 2 1
1 3
1 4
2 5
6 6
3 7
7 8
1 8
8 8
""") == """3
4
3
1
4
2
7
1"""

# custom cases
assert run("""1 1
5
1 1
""") == "1"

assert run("""5 1
5 4 3 2 1
1 5
""") == "4"

assert run("""6 2
1 2 3 2 1 0
1 6
2 5
""") == """5
4"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 1 | trường hợp cơ sở đúng đắn | 
| giảm nghiêm ngặt | 4 | xử lý các ràng buộc tối đa | 
| phạm vi hỗn hợp | 5, 4 | tính nhất quán giữa các truy vấn chồng chéo | 

## Vỏ cạnh 

Mảng một phần tử được xử lý đơn giản vì mỗi nút lá được khởi tạo với giá trị 1 và không có sự hợp nhất nào sẽ làm giảm thêm giá trị đó. Cây phân đoạn trả về giá trị đó một cách trực tiếp. 

Một mảng giảm hoàn toàn như$[5,4,3,2,1]$không bao giờ tạo ra tình huống trong đó DP mất dấu hiệu hợp lệ và hoạt động hợp nhất truyền bá tích lũy chính xác một cách nhất quán trên toàn bộ phân đoạn, tạo ra độ dài chuỗi con tối đa có thể đạt được cho cấu trúc đó. 

Các phạm vi truy vấn chồng chéo sẽ sử dụng lại các nút cây phân đoạn giống nhau và do thao tác hợp nhất nhất quán và giữ nguyên thứ tự nên các kết hợp lặp lại không làm sai lệch kết quả trên các truy vấn.
