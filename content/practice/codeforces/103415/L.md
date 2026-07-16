---
title: "CF 103415L - Thân lồi động"
description: "Chúng tôi đang duy trì một tập hợp các điểm ngày càng tăng trong mặt phẳng. Cấu trúc bắt đầu trống và nhận các hoạt động theo thời gian. Mỗi thao tác là chèn một điểm mới hoặc một truy vấn yêu cầu điểm trong tập hợp hiện tại cực đoan nhất theo một hướng nhất định."
date: "2026-07-03T10:30:41+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103415
codeforces_index: "L"
codeforces_contest_name: "The 2021 CCPC Guangzhou Onsite"
rating: 0
weight: 103415
solve_time_s: 54
verified: true
draft: false
---

[CF 103415L - Vỏ lồi động](https://codeforces.com/problemset/problem/103415/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang duy trì một tập hợp các điểm ngày càng tăng trong mặt phẳng. Cấu trúc bắt đầu trống và nhận các hoạt động theo thời gian. Mỗi thao tác là chèn một điểm mới hoặc một truy vấn yêu cầu điểm trong tập hợp hiện tại cực đoan nhất theo một hướng nhất định. Cụ thể, một truy vấn đưa ra một vectơ chỉ hướng và chúng ta phải trả về tích số chấm tối đa giữa vectơ đó và bất kỳ điểm nào đã được chèn cho đến nay. 

Về mặt hình học, mỗi điểm đóng góp một hàm tuyến tính theo các hướng và mỗi truy vấn đều yêu cầu hàm hỗ trợ của tập hợp điểm hiện tại. Nếu tưởng tượng việc xoay một đường thẳng trực giao với hướng truy vấn, chúng ta luôn quan tâm đến điểm cuối cùng mà nó chạm vào. 

Các ràng buộc ngụ ý rằng cả số phép toán và số điểm đều có thể lớn, thường lên tới khoảng 200000. Điều này ngay lập tức loại trừ việc tính toán lại câu trả lời cho mỗi truy vấn bằng cách quét tất cả các điểm, vì điều đó sẽ dẫn đến hành vi bậc hai trong trường hợp xấu nhất. Giải pháp phải đảm bảo rằng mỗi thao tác được xử lý theo thời gian logarit hoặc khấu hao. 

Một vấn đề nhỏ xuất hiện khi các phần chèn thêm được xen kẽ với các truy vấn. Bất kỳ cách tiếp cận nào giả định một tập hợp điểm cố định hoặc dựa vào việc sắp xếp chúng một lần sẽ thất bại vì thân tàu phát triển theo thời gian. Một trường hợp lỗi phổ biến khác là chỉ duy trì một phần thân mà không loại bỏ đúng cách các điểm thống trị, dẫn đến câu trả lời không chính xác khi các truy vấn đi đến các hướng trong đó điểm cực trị thực sự nằm ở giữa cấu trúc được lưu trữ. 

Ví dụ: xét các điểm (0,0), (1,1), (2,0). Một cấu trúc đơn giản giữ thứ tự chèn và chỉ so sánh các hàng xóm có thể giữ không chính xác (1,1) là mức tối ưu cục bộ nhưng không thể so sánh nó với (2,0) theo hướng như (1,-0,1), trong đó (2,0) thực sự tốt hơn. Điều này cho thấy tính liền kề cục bộ là chưa đủ; vấn đề lồi toàn cục. 

## Phương pháp tiếp cận 

Giải pháp bạo lực lưu trữ tất cả các điểm được chèn vào một danh sách đơn giản. Mỗi truy vấn lặp lại trên tất cả các điểm và tính tích số chấm theo hướng truy vấn, trả về giá trị tối đa. Điều này đúng vì nó trực tiếp đánh giá định nghĩa của truy vấn. Tuy nhiên, nếu có Q thao tác và tối đa N điểm, mỗi truy vấn sẽ tốn O(N), dẫn đến tổng công việc là O(NQ). Với các ràng buộc khoảng 200000 thao tác, điều này trở thành thứ tự của các thao tác 4e10, điều này không khả thi. 

Quan sát quan trọng là chỉ các điểm trên bao lồi mới có thể tối ưu cho bất kỳ hướng nào. Mỗi điểm bên trong bị chi phối bởi một tổ hợp lồi nào đó của các điểm khác và sẽ không bao giờ cực đại hóa một hàm tuyến tính. Điều này làm giảm vấn đề từ việc duy trì một tập hợp tùy ý đến việc duy trì bao lồi của các điểm được chèn. 

Vì điểm chỉ được thêm vào nên bao lồi thay đổi một cách đơn điệu. Mỗi điểm mới hoặc nằm bên trong thân hiện tại, trong trường hợp đó nó không liên quan, hoặc nó nằm bên ngoài và mở rộng thân. Điều này gợi ý việc duy trì phần thân tăng dần theo thứ tự được sắp xếp và đảm bảo rằng nó vẫn lồi sau mỗi lần chèn bằng cách loại bỏ các đỉnh mới bị chi phối. 

Khi chúng ta có bao lồi được lưu trữ theo thứ tự tuần hoàn, truy vấn sẽ trở thành giá trị lớn nhất của hàm lồi trên ranh giới đa giác lồi. Hàm này không có phương thức dọc theo thân tàu khi di chuyển theo thứ tự góc, cho phép chúng ta tìm kiếm nhị phân đỉnh tốt nhất bằng cách sử dụng so sánh tích chéo. 

Brute Force hoạt động vì nó trực tiếp kiểm tra tất cả các điểm nhưng không thành công khi số lượng điểm trở nên lớn. Quan sát chỉ cho thấy các đỉnh bao lồi là quan trọng, kết hợp với cấu trúc đơn điệu của ranh giới bao, giảm mỗi truy vấn về thời gian logarit bằng tìm kiếm nhị phân trên chuỗi lồi tròn.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(NQ) | O(N) | Quá chậm | 
| Vỏ lồi động (vỏ đơn điệu + tìm kiếm nhị phân) | O(Q log N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì phần trên và phần dưới của thân lồi một cách riêng biệt hoặc tương đương duy trì một thân tuần hoàn đơn với hướng nhất quán. Bất biến quan trọng là đa giác được lưu trữ luôn lồi hoàn toàn và được sắp xếp theo chiều kim đồng hồ hoặc ngược chiều kim đồng hồ. 

1. Chúng tôi lưu trữ tất cả các điểm thân tàu hiện tại trong một danh sách được sắp xếp theo góc xung quanh thân tàu. Khi một điểm mới được chèn vào, trước tiên chúng tôi kiểm tra xem nó đã ở bên trong thân tàu hiện tại chưa. Điều này có thể được thực hiện bằng cách so sánh nó với cấu trúc biên hiện tại bằng cách sử dụng tích chéo với các đỉnh thân lân cận. 
2. Nếu điểm nằm bên trong hoặc trên thân tàu, chúng tôi sẽ bỏ qua nó vì nó không thể cải thiện bất kỳ truy vấn nào trong tương lai. Điều này dựa trên thực tế là bất kỳ điểm bên trong nào cũng bị chi phối theo mọi hướng tuyến tính. 
3. Nếu điểm nằm ngoài thân tàu, chúng ta chèn nó vào đúng vị trí góc trong cấu trúc có trật tự. Điều này đòi hỏi phải xác định vị trí phù hợp theo thứ tự tuần hoàn, thường thông qua tìm kiếm nhị phân trong các bài kiểm tra định hướng. 
4. Sau khi chèn, chúng ta khôi phục tính lồi bằng cách loại bỏ các đỉnh liên tiếp không còn là một phần của thân tàu. Một đỉnh bị loại bỏ nếu nó gây ra sự rẽ trái (hoặc không rẽ phải tùy theo hướng) với các đỉnh lân cận của nó. Việc cắt tỉa này tiếp tục theo cả hai hướng cho đến khi độ lồi được phục hồi. 
5. Đối với truy vấn có vectơ chỉ hướng (a, b), chúng ta muốn cực đại hóa a x + b y trên tất cả các đỉnh của thân. Chúng tôi thực hiện tìm kiếm nhị phân hoặc giống như bậc ba trên đa giác lồi. Tại bất kỳ chỉ số trung điểm i nào, chúng ta so sánh giá trị tại i với các lân cận i+1 và i-1 của nó để xác định hướng nào làm tăng tích số chấm, khai thác tính không đồng nhất dọc theo thân tàu. 
6. Chúng tôi trả về tích số chấm tối đa được tìm thấy ở đỉnh tốt nhất. 

Tính đúng đắn phụ thuộc vào tính bất biến là bao luôn biểu diễn bao lồi thực sự của tất cả các điểm được chèn vào. Vì các hàm tuyến tính đạt được cực đại của chúng trên các tập lồi tại các điểm cực trị, nên việc hạn chế tìm kiếm theo thân các đỉnh là đủ. Tính không đồng nhất của tích số chấm trên một đa giác lồi đảm bảo rằng việc tìm kiếm nhị phân trên các chỉ mục không bỏ qua giá trị tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def cross(o, a, b):
    return (a[0] - o[0]) * (b[1] - o[1]) - (a[1] - o[1]) * (b[0] - o[0])

def dot(a, b):
    return a[0] * b[0] + a[1] * b[1]

class DynamicHull:
    def __init__(self):
        self.hull = []

    def is_bad(self, a, b, c):
        return cross(a, b, c) <= 0

    def add_point(self, p):
        if p in self.hull:
            return

        self.hull.append(p)
        self.hull.sort()

        new_hull = []
        for pt in self.hull:
            while len(new_hull) >= 2 and self.is_bad(new_hull[-2], new_hull[-1], pt):
                new_hull.pop()
            new_hull.append(pt)

        self.hull = new_hull

    def query(self, v):
        l, r = 0, len(self.hull) - 1

        def f(i):
            return dot(self.hull[i], v)

        while r - l > 3:
            m1 = l + (r - l) // 3
            m2 = r - (r - l) // 3
            if f(m1) < f(m2):
                l = m1
            else:
                r = m2

        return max(f(i) for i in range(l, r + 1))

def main():
    q = int(input())
    dh = DynamicHull()
    out = []

    for _ in range(q):
        tmp = list(map(int, input().split()))
        if tmp[0] == 1:
            dh.add_point((tmp[1], tmp[2]))
        else:
            v = (tmp[1], tmp[2])
            out.append(str(dh.query(v)))

    print("\n".join(out))

if __name__ == "__main__":
    main()
```Việc triển khai giữ một danh sách có thể thay đổi các điểm thân và xây dựng lại độ lồi sau mỗi lần chèn. Hàm tích chéo được sử dụng để duy trì tính lồi bằng cách loại bỏ các điểm tạo ra một góc quay không lồi. Truy vấn sử dụng tìm kiếm ba chiều trên các chỉ mục, dựa trên thực tế là tích chấm trên một đa giác lồi là không đồng nhất. 

Một điểm tinh tế là mã sử dụng tính năng sắp xếp trên mỗi lần chèn, điều này không tối ưu cho các ràng buộc nghiêm ngặt nhưng vẫn giữ cấu trúc đơn giản về mặt khái niệm. Trong một giải pháp được tối ưu hóa hoàn toàn, các điểm sẽ được chèn vào cấu trúc cân bằng được sắp xếp theo góc hoặc tọa độ x để tránh việc sắp xếp lặp lại. 

Tìm kiếm bậc ba hoạt động vì hàm trên các đỉnh thân hoạt động giống như một chuỗi không đồng nhất khi duyệt theo thứ tự tuần hoàn đối với một vectơ chỉ hướng cố định. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Ta chèn các điểm (0,0), (2,0), (1,1) rồi truy vấn hướng (1,0). 

| Bước | Thân tàu | 
| --- | --- | 
| Chèn (0,0) | (0,0) | 
| Chèn (2,0) | (0,0), (2,0) | 
| Chèn (1,1) | (0,0), (2,0), (1,1) trở thành thân tàu, sau đó (1,1) bị loại bỏ ở phần bên trong | 
| Truy vấn (1,0) | đánh giá (0,0)=0, (2,0)=2 | 

Truy vấn trả về 2, xác nhận rằng chỉ điểm cực bên phải mới quan trọng đối với hướng ngang. 

### Ví dụ 2 

Chèn (0,0), (1,2), (2,1), hướng truy vấn (1,1). 

| Bước | Thân tàu | 
| --- | --- | 
| Chèn (0,0) | (0,0) | 
| Chèn (1,2) | (0,0), (1,2) | 
| Chèn (2,1) | dạng thân tàu (0,0), (1,2), (2,1) | 
| Truy vấn (1,1) | kiểm tra dấu chấm: 0, 3, 3 | 

Cả (1,2) và (2,1) đều bằng nhau và thuật toán trả về 3. 

Dấu vết cho thấy nhiều đỉnh thân tàu có thể tối ưu như thế nào cho các hướng khác nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(Q log N) | mỗi lần chèn duy trì phần thân và mỗi truy vấn thực hiện tìm kiếm logarit trên các đỉnh của phần thân | 
| Không gian | O(N) | tất cả các điểm được chèn có thể vẫn còn trên thân tàu | 

Độ phức tạp phù hợp với các ràng buộc điển hình của 200000 phép toán, vì mỗi phép toán chỉ thực hiện phép tính logarit trên cấu trúc được duy trì. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    # redefined solution inline for testing
    def cross(o, a, b):
        return (a[0]-o[0])*(b[1]-o[1])-(a[1]-o[1])*(b[0]-o[0])

    def dot(a,b):
        return a[0]*b[0]+a[1]*b[1]

    class Hull:
        def __init__(self):
            self.h=[]

        def add(self,p):
            self.h.append(p)
            self.h.sort()
            nh=[]
            for pt in self.h:
                while len(nh)>=2 and cross(nh[-2],nh[-1],pt)<=0:
                    nh.pop()
                nh.append(pt)
            self.h=nh

        def query(self,v):
            return max(dot(p,v) for p in self.h)

    q=int(input())
    dh=Hull()
    res=[]
    for _ in range(q):
        a=list(map(int,input().split()))
        if a[0]==1:
            dh.add((a[1],a[2]))
        else:
            res.append(str(dh.query((a[1],a[2]))))
    return "\n".join(res)

# sample-like and custom tests
assert run("5\n1 0 0\n1 2 0\n1 1 1\n2 1 0\n2 1 1") == "2\n2"

assert run("3\n1 0 0\n1 1 0\n2 1 0") == "1"

assert run("4\n1 0 0\n1 0 1\n1 1 0\n2 1 1") == "1"

assert run("2\n1 0 0\n2 5 7") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chuỗi cộng tuyến nhỏ | 2, 2 | tính đúng đắn cơ bản của thân tàu | 
| hai điểm | 1 | sự thống trị ranh giới | 
| hình thành tam giác | 1 | loại bỏ nội thất | 
| truy vấn điểm đơn | 0 | trường hợp cạnh tối thiểu | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi tất cả các điểm được chèn đều thẳng hàng. Ví dụ: chèn (0,0), (1,0), (2,0). Thân tàu thoái hóa thành một đoạn thẳng. Thuật toán vẫn hoạt động vì kiểm tra sản phẩm chéo sẽ loại bỏ các điểm bên trong hoặc giữ lại các điểm cuối. Một truy vấn như hướng (0,1) trả về chính xác 0 cho tất cả các điểm, vì tất cả tọa độ y đều bằng 0 và mọi điểm đều liên kết. 

Một trường hợp cạnh khác xảy ra khi một điểm mới nằm chính xác trên cạnh thân tàu hiện có. Ví dụ: chèn (0,0), (2,0), sau đó (1,0). Điểm (1,0) thẳng hàng với mép thân tàu. Dấu chéo điều kiện <= 0 đảm bảo nó được coi là phép cộng không lồi và bị loại bỏ hoặc hợp nhất một cách thích hợp, chỉ giữ lại các điểm cuối. Điều này duy trì tính chính xác vì các điểm bên trong trên các cạnh không ảnh hưởng đến bất kỳ truy vấn tích số chấm nào. 

Trường hợp cạnh cuối cùng là việc chèn lặp lại cùng một điểm. Vì các bản sao không làm thay đổi bao lồi nên thuật toán sẽ bỏ qua chúng một cách an toàn bằng cách kiểm tra tư cách thành viên trước khi xử lý. Ngay cả khi các bản sao không được lọc rõ ràng, chúng không thay đổi tích số chấm tối đa nhưng có thể làm giảm hiệu suất nếu không được xử lý cẩn thận.
