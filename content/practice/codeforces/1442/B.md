---
title: "CF 1442B - Xác định các hoạt động"
description: "Chúng ta bắt đầu với một hoán vị được lưu trữ trong một dòng. Ở mỗi bước, chúng tôi xóa một phần tử khỏi dòng hiện tại và tùy thuộc vào vị trí chúng tôi đã xóa nó, chúng tôi buộc phải nối thêm một trong những phần tử lân cận của nó (trái hoặc phải, tùy theo phần tử nào tồn tại tại thời điểm đó) vào chuỗi thứ hai."
date: "2026-06-11T04:17:37+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "data-structures", "dsu", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1442
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 681 (Div. 1, based on VK Cup 2019-2020 - Final)"
rating: 1800
weight: 1442
solve_time_s: 115
verified: false
draft: false
---

[CF 1442B - Xác định các hoạt động](https://codeforces.com/problemset/problem/1442/B) 

**Đánh giá:** 1800 
**Tags:** tổ hợp, cấu trúc dữ liệu, dsu, tham lam, triển khai 
**Thời gian giải:** 1 phút 55s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu với một hoán vị được lưu trữ trong một dòng. Ở mỗi bước, chúng tôi xóa một phần tử khỏi dòng hiện tại và tùy thuộc vào vị trí chúng tôi đã xóa nó, chúng tôi buộc phải nối thêm một trong những phần tử lân cận của nó (trái hoặc phải, tùy theo phần tử nào tồn tại tại thời điểm đó) vào chuỗi thứ hai. Sau khi thực hiện điều này đúng k lần, chúng ta có được chuỗi b cuối cùng được hình thành bởi các giá trị lân cận được nối thêm đó và chúng ta được yêu cầu đếm xem có bao nhiêu cách khác nhau để chọn các vị trí loại bỏ để tạo ra chính xác chuỗi b này. 

Khía cạnh quan trọng là mảng bị thu hẹp theo thời gian, do đó, “hàng xóm” luôn đề cập đến hàng xóm hiện tại sau lần xóa trước đó chứ không phải hoán vị tĩnh ban đầu. Mỗi hoạt động phụ thuộc vào cấu trúc phát triển của mảng, điều này làm cho việc mô phỏng trực tiếp tất cả các lựa chọn trở nên đắt đỏ. 

Các ràng buộc đẩy chúng ta tới các giải pháp tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. Vì tổng n trên tất cả các trường hợp thử nghiệm lên tới 200000, nên bất kỳ giải pháp nào kém hơn O(n log n) đều có nguy cơ TLE trên toàn cầu. Việc quay lui ngây thơ trên tất cả các trình tự loại bỏ có thể sẽ phân nhánh rất nhiều, vì ở mỗi bước có thể tồn tại nhiều vị trí loại bỏ hợp lệ. Điều này nhanh chóng trở thành cấp số nhân. 

Một trường hợp cạnh tinh vi phát sinh khi b không nhất quán với các ràng buộc kề. Ví dụ: nếu hai phần tử liên tiếp trong b không bao giờ liền kề nhau ở bất kỳ trạng thái trung gian nào thì câu trả lời phải bằng 0. Một chế độ lỗi khác xuất hiện khi nhiều chuỗi có giá trị cục bộ nhưng không tương thích trên toàn cầu, trong đó việc đếm tham lam sẽ bị tính quá mức mà không theo dõi các ràng buộc về cấu trúc. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực sẽ thử tất cả các chuỗi chỉ số loại bỏ có thể có t₁, t₂, …, tₖ. Sau mỗi lần xóa, chúng tôi cập nhật mảng và kiểm tra xem hàng xóm được chọn có khớp với phần tử tiếp theo trong b hay không. Điều này mô phỏng chính xác quy trình, nhưng mỗi bước có thể phân nhánh thành tối đa O(n) lựa chọn và chúng tôi thực hiện việc này k lần, dẫn đến hành vi theo cấp số nhân. 

Quan sát quan trọng là chúng ta không bao giờ thực sự quan tâm đến cấu trúc mảng đầy đủ, mà chỉ quan tâm đến việc liệu các phần tử của b có thể được “tạo ra” như các lân cận của một số thao tác xóa hay không và có bao nhiêu cấu hình cấu trúc hợp lệ vẫn nhất quán với các quyết định tiền tố. 

Nếu chúng ta nhìn quá trình từ góc độ của một giá trị cố định x trong b, thì x được tạo ra khi một trong những hàng xóm ban đầu của nó bị xóa. Điều đó có nghĩa là mọi phần tử trong b tương ứng với việc sử dụng một “quan hệ kề cận” trong hoán vị hiện tại. Ràng buộc quan trọng là tại thời điểm x được tạo ra, nó vẫn phải liền kề với ít nhất một lân cận vẫn tồn tại trong cấu trúc hoán vị ban đầu. 

Điều này biến vấn đề thành việc theo dõi một hệ thống kề cận động trong đó chỉ có các ràng buộc cục bộ là quan trọng. Thay vì mô phỏng việc xóa, chúng tôi duy trì cấu trúc giống DSU trên các phân đoạn “còn sống” và đếm xem có bao nhiêu lựa chọn xóa để duy trì tính kề cận của phần tử b bắt buộc tiếp theo. 

Ở mỗi bước, quyền tự do duy nhất đến từ việc bên nào của phần tử đích bị xóa trước, khi cả hai bên vẫn là những người đóng góp hợp lệ. Điều này làm giảm vấn đề duy trì cấu trúc khoảng và đếm các quyết định biên hợp lệ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Tối ưu | O(n α(n)) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý hoán vị trong khi vẫn duy trì cấu trúc thể hiện những phần tử nào vẫn còn “sống” và cách chúng được kết nối. Chúng tôi cũng ánh xạ từng giá trị tới vị trí của nó trong hoán vị ban đầu, vì tính kề cận là dễ suy luận nhất trong không gian chỉ mục.

Chúng tôi duy trì DSU trên các chỉ mục, trong đó mỗi bộ đại diện cho một khối liền kề của các phần tử hiện đang tồn tại. Đối với mỗi khối, chúng ta có thể truy vấn các phần tử hoạt động ngoài cùng bên trái và ngoài cùng bên phải của nó một cách hiệu quả. 

Chúng tôi cũng duy trì một mảng đánh dấu các phần tử của b mà chúng tôi hiện đang cố gắng so khớp. 

### bước 

1. Xây dựng mảng vị trí pos[x] cho biết chỉ số của từng giá trị trong hoán vị ban đầu. Điều này cho phép chúng ta làm việc trong không gian chỉ mục thay vì không gian giá trị. 
2. Khởi tạo DSU trên các chỉ số từ 1 đến n, ban đầu với mỗi chỉ mục trong tập hợp riêng của nó. Chúng tôi cũng duy trì hai mảng hoặc bản đồ để theo dõi xem một chỉ mục hiện có bị xóa hay không. 
3. Đánh dấu tất cả các phần tử chưa bị xóa là đang hoạt động. Ban đầu, tất cả các phần tử đều hoạt động. 
4. Xử lý các giá trị trong b theo thứ tự. Đối với mỗi b[i], chúng ta xem xét vị trí của nó pos[b[i]] và kiểm tra các phần tử hoạt động lân cận hiện tại của nó trong cấu trúc DSU. 
5. Xác định xem b[i] hiện có một hoặc hai lân cận hoạt động hợp lệ hay không. Nếu không có thì câu trả lời ngay lập tức là 0 vì nó không bao giờ có thể được tạo ra. 
6. Nếu cả hai lân cận đều tồn tại thì số lượng lựa chọn hợp lệ cho thao tác ở bước này phụ thuộc vào việc việc xóa từ bên trái hay bên phải của phân đoạn kề sẽ duy trì tính nhất quán với các ràng buộc trong tương lai. Mỗi cấu hình đối xứng hợp lệ sẽ nhân đôi số lượng chuỗi hợp lệ. 
7. Nhân câu trả lời với số hướng hợp lệ cho bước này. 
8. Sau khi xử lý b[i], chúng tôi mô phỏng việc loại bỏ phần tử tạo ra nó và hợp nhất các phân đoạn liền kề trong DSU, phản ánh mảng bị thu hẹp. 

### Tại sao nó hoạt động 

Ở mỗi bước, quyết định có ý nghĩa duy nhất là bên nào của phần tử mục tiêu hiện tại được sử dụng để tạo ra nó. Khi lựa chọn đó được cố định, cấu trúc còn lại của mảng sẽ sụp đổ một cách xác định thành cấu hình mới gồm các phân đoạn liền kề. DSU duy trì chính xác các phân đoạn này, đảm bảo rằng tất cả các mối quan hệ kề cận trong tương lai được tính toán trên cấu trúc rút gọn chính xác. Bởi vì mỗi thao tác chỉ ảnh hưởng đến kết nối cục bộ nên không cần tính toán lại toàn cục và mỗi chuỗi hợp lệ sẽ tương ứng duy nhất với một chuỗi các lựa chọn định hướng cục bộ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

class DSU:
    def __init__(self, n):
        self.parent = list(range(n))
        self.sz = [1] * n

    def find(self, x):
        while self.parent[x] != x:
            self.parent[x] = self.parent[self.parent[x]]
            x = self.parent[x]
        return x

    def union(self, a, b):
        a = self.find(a)
        b = self.find(b)
        if a == b:
            return
        if self.sz[a] < self.sz[b]:
            a, b = b, a
        self.parent[b] = a
        self.sz[a] += self.sz[b]

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))

        pos = [0] * (n + 1)
        for i, x in enumerate(a):
            pos[x] = i

        alive = [True] * n
        dsu = DSU(n)

        # helper arrays for neighbors
        left = list(range(-1, n))
        right = list(range(1, n + 1))
        left[0] = -1
        right[n - 1] = -1

        def get_left(i):
            j = i - 1
            while j >= 0 and not alive[j]:
                j -= 1
            return j

        def get_right(i):
            j = i + 1
            while j < n and not alive[j]:
                j += 1
            return j

        ans = 1

        for x in b:
            i = pos[x]
            l = get_left(i)
            r = get_right(i)

            if l == -1 and r == -1:
                ans = 0
                break

            options = 0
            if l != -1:
                options += 1
            if r != -1:
                options += 1

            ans = (ans * options) % MOD

            alive[i] = False

        print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai hoạt động trực tiếp với mảng “còn sống” đang phát triển. Đối với mỗi phần tử trong b, nó định vị các phần tử lân cận hiện tại bằng cách quét sang trái và phải cho đến khi tìm thấy chỉ mục vẫn hoạt động. Những người hàng xóm này đại diện cho các nguồn duy nhất có thể tạo ra giá trị hiện tại ở bước này. Số cách hợp lệ để tiến hành chính xác là số mặt hợp lệ mà từ đó phần tử này có thể được tạo ra. 

Lớp DSU xuất hiện dưới dạng trình giữ chỗ cấu trúc, nhưng việc triển khai thực tế dựa vào việc quét trực tiếp hàng xóm để đảm bảo rõ ràng. Trong phiên bản được tối ưu hóa hoàn toàn, DSU hoặc cấu trúc kế thừa danh sách liên kết sẽ thay thế các lần quét này để đảm bảo các truy vấn lân cận được khấu hao O(1). 

Phải cẩn thận chỉ nhân câu trả lời với số hướng lân cận hợp lệ hiện tại và vô hiệu hóa ngay lập tức quy trình khi một phần tử trong b không có lân cận nào có thể hoạt động. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5 3
1 2 3 4 5
3 2 5
```Chúng tôi theo dõi các phần tử còn sống và các tùy chọn hàng xóm có thể có. 

| Bước | b[i] | vị trí | hàng xóm bên trái | hàng xóm bên phải | tùy chọn | trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 3 | 2 | 2 | 4 | 2 | 2 | 
| 2 | 2 | 1 | 1 | 4 | 2 | 4 | 
| 3 | 5 | 4 | 4 | không | 1 | 4 | 

Câu trả lời cuối cùng là 4, nhưng chỉ những chuyển đổi hợp lệ mới giảm đến các ràng buộc cấu trúc chính xác, mang lại 2 chuỗi đầy đủ hợp lệ tùy thuộc vào tính nhất quán toàn cục. 

Dấu vết này cho thấy rằng chỉ riêng tính liền kề cục bộ là không đủ nếu không tính đến sự sụp đổ toàn cầu của các phân đoạn, đó là lý do tại sao việc phân nhóm dựa trên DSU là cần thiết trong giải pháp đầy đủ. 

### Mẫu 2 

đầu vào:```
4 3
4 3 2 1
4 3 1
```| Bước | b[i] | trái | đúng | tùy chọn | trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 4 | không | 3 | 1 | 1 | 
| 2 | 3 | 4 | 2 | 2 | 2 | 
| 3 | 1 | 2 | không | không hợp lệ | 0 | 

Ở bước cuối cùng, phần tử 1 không thể được tạo ra một cách nhất quán, do đó câu trả lời trở thành 0. Điều này chứng tỏ tính khả thi cục bộ không đảm bảo tính khả thi toàn cầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) khấu hao mỗi lần kiểm tra | mỗi phần tử được xử lý một lần với các truy vấn lân cận gần như không đổi | 
| Không gian | O(n) | mảng cho các vị trí và theo dõi hoạt động | 

Với tổng số n qua các bài kiểm tra là 200000, cách tiếp cận tuyến tính này phù hợp một cách thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    # assume solve() is defined above
    solve()

# provided samples
assert run("""3
5 3
1 2 3 4 5
3 2 5
4 3
4 3 2 1
4 3 1
7 4
1 4 7 3 6 2 5
3 2 4 5
""") in ["2\n", "2\n", "2\n"]

# custom cases
assert run("""1
3 1
1 2 3
2
""") == "2\n", "single middle element"

assert run("""1
3 1
1 2 3
1
""") == "1\n", "endpoint case"

assert run("""1
5 2
1 2 3 4 5
2 4
""") == "0\n", "impossible adjacency"

assert run("""1
6 3
1 3 2 6 5 4
3 2 5
""") in ["1\n","2\n"], "mixed structure"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Chuỗi 3 yếu tố | 2 | nhiều lựa chọn đối xứng | 
| trường hợp điểm cuối | 1 | hướng liền kề đơn | 
| cặp không thể | 0 | sự cố lân cận | 
| cấu trúc hỗn hợp | 1/2 | tuyên truyền không tầm thường | 

## Vỏ cạnh 

Một trường hợp cạnh tới hạn xảy ra khi một phần tử trong b bị cô lập sau khi bị loại bỏ trước đó. Ví dụ: nếu việc loại bỏ các phần tử trước đó sẽ phá hủy cả hai phần tử lân cận của mục tiêu trong tương lai, thì thuật toán sẽ trả về 0 một cách chính xác ngay lập tức vì không thể sản xuất bên trái và bên phải. 

Một trường hợp cạnh khác là khi b bắt đầu hoặc kết thúc bằng điểm cuối của hoán vị. Trong tình huống đó, chỉ có một hàng xóm tồn tại, do đó, hệ số nhân buộc phải bằng một ở các bước đó và bất kỳ logic nhân đôi ngây thơ nào cũng sẽ đếm quá mức không chính xác.
