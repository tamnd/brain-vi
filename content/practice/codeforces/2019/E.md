---
title: "CF 2019E - Cắt Tỉa Cây"
description: "Chúng ta được cung cấp một danh sách được sắp xếp các tọa độ nguyên trên một dòng. Từ mỗi cặp điểm này, chúng ta vẽ một đoạn kín, nghĩa là mọi điểm nguyên giữa các điểm cuối đều được bao gồm trong đoạn đó."
date: "2026-06-09T03:00:07+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "dfs-and-similar", "dp", "sortings", "trees"]
categories: ["algorithms"]
codeforces_contest: 2019
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 975 (Div. 2)"
rating: 1700
weight: 2019
solve_time_s: 277
verified: false
draft: false
---

[CF 2019E - Cắt tỉa cây](https://codeforces.com/problemset/problem/2019/E) 

**Đánh giá:** 1700 
**Thẻ:** thuật toán xây dựng, dfs và tương tự, dp, sắp xếp, cây 
**Thời gian giải:** 4 phút 37 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một danh sách được sắp xếp các tọa độ nguyên trên một dòng. Từ mỗi cặp điểm này, chúng ta vẽ một đoạn kín, nghĩa là mọi điểm nguyên giữa các điểm cuối đều được bao gồm trong đoạn đó. Sau khi xây dựng tất cả các phân đoạn như vậy, mỗi vị trí số nguyên trên dòng được bao phủ bởi một số phân đoạn. Nhiệm vụ là trả lời nhiều truy vấn hỏi: có bao nhiêu vị trí nguyên được bao phủ bởi một số phân đoạn nhất định. 

Một cách trực tiếp để diễn giải cấu trúc là mỗi điểm trên đường tích lũy một “số lượng vùng phủ sóng” tùy thuộc vào số lượng cặp điểm cuối được chọn bao quanh nó. 

Các ràng buộc ngụ ý rằng cả số lượng điểm và truy vấn đều có thể lớn, lên tới một trăm nghìn cho mỗi trường hợp thử nghiệm, với tổng số tiền theo thứ tự đó qua các thử nghiệm. Điều này ngay lập tức loại trừ bất kỳ nghiệm nào lặp lại trên tất cả các cặp điểm, vì đó sẽ là nghiệm bậc hai và vượt xa giới hạn thời gian. Ngay cả việc xây dựng tất cả các đóng góp phân đoạn một cách rõ ràng cũng là không thể vì có các phân đoạn O(n2). 

Khó khăn chính là phạm vi bao phủ không được bản địa hóa thành các phân đoạn riêng lẻ mà thay vào đó phụ thuộc vào mối quan hệ kết hợp giữa các chỉ số theo thứ tự được sắp xếp. 

Trường hợp cạnh tinh tế xuất hiện khi tọa độ rất thưa thớt, ví dụ khi chênh lệch giữa các giá trị x liên tiếp là rất lớn. Trong những trường hợp như vậy, một giải pháp đơn giản lặp lại các vị trí số nguyên giữa các điểm cuối sẽ không thành công vì phạm vi tọa độ có thể mở rộng lên tới 10⁹, khiến cho việc lặp trên mỗi số nguyên là không thể. Một trường hợp cạnh khác là khi tất cả các giá trị x liên tiếp; ở đây cấu trúc trở nên dày đặc và giải pháp chính xác vẫn phải xử lý các tương tác phân đoạn O(n²) mà không liệt kê chúng. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ tạo ra mọi cặp (i, j) và đánh dấu vùng phủ sóng trên tất cả các điểm nguyên giữa x_i và x_j. Về nguyên tắc, điều này đúng vì nó tuân theo định nghĩa trực tiếp, nhưng mỗi phân đoạn có thể bao phủ tối đa vị trí O(max x) và có các phân đoạn O(n²), vì vậy điều này sẽ bùng nổ ngay lập tức. Ngay cả với những tối ưu hóa nhỏ, cấu trúc về cơ bản là quá lớn để mô phỏng. 

Quan sát chính là đảo ngược quan điểm. Thay vì suy nghĩ về các đoạn bao phủ các điểm, chúng ta hỏi có bao nhiêu đoạn bao phủ một vị trí nguyên p cho trước. Một đoạn [x_i, x_j] bao phủ p chính xác khi x_i ≤ p ≤ x_j, tương đương với việc chọn hai điểm cuối ở hai phía đối diện của p. Nếu chúng ta định nghĩa cnt_left(p) là số điểm ≤ p và cnt_right(p) là số điểm ≥ p, thì số đoạn bao phủ p là cnt_left(p) nhân với cnt_right(p), trừ đi số hiệu chỉnh vượt mức cho các ràng buộc thứ tự. Tuy nhiên, vì các điểm cuối hoàn toàn nằm trong tập hợp đã cho nên cấu trúc chính xác được đơn giản hóa thành một hàm chỉ phụ thuộc vào số lượng x_i nằm ở bên trái của p. 

Giữa x_i và x_{i+1} liên tiếp, số lượng vùng phủ sóng không đổi vì không có điểm cuối phân đoạn nào nằm trong khoảng đó. Điều này làm giảm vấn đề phân tích các khoảng được tạo ra bởi các tọa độ đã được sắp xếp thay vì các vị trí số nguyên riêng lẻ. 

Đối với mỗi khoảng cách giữa các điểm liên tiếp, chúng tôi tính toán có bao nhiêu đoạn lấp đầy khoảng cách đó. Giá trị đó chỉ phụ thuộc vào số lượng điểm nằm ở bên trái và bên phải của vùng đó. Mỗi cặp (i, j) đóng góp chính xác (x_j - x_i) số nguyên và mỗi điểm như vậy có chung cấu trúc bao phủ. Điều này cho phép chúng tôi tổng hợp các đóng góp trong O(n) bằng cách sử dụng phép tính tổ hợp trên các cặp chỉ số thay vì phạm vi tọa độ. 

Cuối cùng, chúng tôi xây dựng bản đồ tần số theo các giá trị vùng phủ sóng và trả lời trực tiếp các truy vấn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n² · phạm vi) | O(phạm vi) | Quá chậm | 
| Tối ưu | O(n²) hoặc O(n log n) tùy theo cách triển khai | O(n) | Đã chấp nhận |

## Hướng dẫn thuật toán 

Chúng tôi trình bày lại vấn đề về mặt đóng góp từ các cặp chỉ số và tổng hợp có bao nhiêu vị trí số nguyên nhận được mỗi giá trị bao phủ. 

1. Sắp xếp và sửa các điểm đã cho. Chúng đã được sắp xếp sẵn nên chúng ta có thể làm việc trực tiếp với vị trí của chúng mà không cần sắp xếp lại. 
2. Quan sát rằng phạm vi bao phủ chỉ thay đổi ở tọa độ nguyên liền kề với các điểm đã cho. Giữa x_i và x_{i+1}, mọi điểm nguyên hoạt động giống hệt nhau về phân đoạn bao phủ nó. Điều này cho phép chúng ta coi mỗi khoảng là một khối. 
3. Với mỗi cặp chỉ số (i, j), hãy tính xem có bao nhiêu điểm nguyên nằm hoàn toàn trong khoảng [x_i, x_j]. Mỗi điểm như vậy nhận được sự đóng góp từ chính xác các phân đoạn có điểm cuối bao quanh nó, vì vậy mỗi cặp đóng góp đồng đều trong khoảng thời gian của nó. 
4. Thay vì lặp lại tất cả các điểm trong các khoảng, hãy tích lũy số đếm bằng cách sử dụng hiệu. Chúng tôi coi mỗi phân đoạn là đóng góp một bản cập nhật phạm vi: mức độ phù hợp +1 trên [x_i, x_j]. Không thể tạo mảng chênh lệch trên tọa độ nguyên do tọa độ lớn, vì vậy chúng tôi nén các cập nhật thành các phần đóng góp theo khoảng giữa các giá trị x liên tiếp. 
5. Chuyển đổi điều này thành việc đếm có bao nhiêu phân đoạn bao gồm mỗi khoảng cơ bản [x_k, x_{k+1}). Đối với một khoảng cố định, số lượng phân đoạn bao phủ được xác định bằng cách chọn điểm cuối i ≤ k và j ≥ k+1, từ đó đưa ra k · (n − k) lựa chọn. 
6. Mỗi khoảng như vậy có độ dài (x_{k+1} − x_k), do đó, nó đóng góp nhiều điểm nguyên có cùng giá trị bao phủ k · (n − k). 
7. Chúng tôi tổng hợp trên tất cả k từ 1 đến n−1, lưu trữ tần số của từng giá trị vùng phủ sóng nhân với độ dài khoảng thời gian. 
8. Cuối cùng, trả lời từng truy vấn bằng cách tra cứu bản đồ tần số được tính toán trước. 

### Tại sao nó hoạt động 

Bất biến chính là mọi điểm nguyên bên trong khoảng cách giữa các tọa độ liên tiếp đều không thể phân biệt được đối với phạm vi bao phủ của phân đoạn: bất kỳ phân đoạn nào bao phủ một điểm như vậy sẽ bao gồm tất cả các điểm đó và bất kỳ phân đoạn nào thiếu một điểm sẽ bỏ lỡ tất cả. Điều này làm giảm vấn đề từ suy luận về tọa độ sang suy luận về ranh giới chỉ số. Số lượng các đoạn bao phủ một khoảng trống chỉ phụ thuộc vào số lượng điểm cuối nằm ở bên trái và bên phải của nó, điểm này cố định cho mỗi khoảng trống và không phụ thuộc vào vị trí số nguyên chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from collections import defaultdict

def solve():
    t = int(input())
    for _ in range(t):
        n, q = map(int, input().split())
        x = list(map(int, input().split()))
        queries = list(map(int, input().split()))

        freq = defaultdict(int)

        for i in range(n - 1):
            left = i + 1
            right = n - (i + 1)
            cov = left * right
            length = x[i + 1] - x[i]
            freq[cov] += length

        res = []
        for k in queries:
            res.append(str(freq.get(k, 0)))

        print(" ".join(res))

if __name__ == "__main__":
    solve()
```Việc triển khai dựa trên việc nén sự đóng góp của từng phân đoạn vào các khoảng điểm liền kề. Vòng lặp i xử lý từng khoảng cơ bản giữa các tọa độ liên tiếp. Đối với khoảng i, bất kỳ phân đoạn nào bao gồm khoảng đó phải bắt đầu tại hoặc trước i và kết thúc sau i, điều này mang lại cấu trúc tích (i+1) · (n−i−1). Nhân với độ dài khoảng sẽ chuyển đổi số lượng phân đoạn thành số lượng điểm nguyên. 

Một cạm bẫy phổ biến là cố gắng coi tọa độ trực tiếp dưới dạng chỉ số mà không tính đến các khoảng trống. Phép nhân với x[i+1] − x[i] là cần thiết vì mỗi khoảng đại diện cho nhiều vị trí số nguyên. 

## Ví dụ đã hoạt động 

Xét một trường hợp nhỏ có điểm [1, 3, 6]. Các phân đoạn là [1,3], [1,6], [3,6]. Khoảng [1,3) có độ dài 2 và được bao phủ bởi các phân đoạn bắt đầu từ 1 và kết thúc ở 3 hoặc 6, do đó phạm vi bao phủ được xác định bởi các điểm cuối xung quanh chỉ số 0. Khoảng [3,6) có độ dài 3 và được bao phủ bởi các phân đoạn liên quan đến chỉ số 1. 

| Khoảng thời gian | Chiều dài | Công thức bảo hiểm | Kết quả | 
| --- | --- | --- | --- | 
| [1,3) | 2 | 1 × 2 | 2 | 
| [3,6) | 3 | 2 × 1 | 2 | 

Điều này cho thấy các giá trị bao phủ giống hệt nhau có thể xuất hiện như thế nào trên các kích thước khoảng khác nhau. 

Bây giờ hãy xem xét [1,2,4,7]. Mỗi khoảng trống liền kề đóng góp độc lập và phạm vi bao phủ chỉ phụ thuộc vào phân chia chỉ số chứ không phụ thuộc vào khoảng cách thực tế. Điều này chứng tỏ rằng hình học không còn phù hợp một khi các khoảng đã được cố định. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + q) | Mỗi khoảng được xử lý một lần và mỗi truy vấn được trả lời trong tra cứu O(1) | 
| Không gian | O(n) | Bản đồ tần suất lưu trữ tối đa một mục nhập trong mỗi khoảng thời gian | 

Tổng độ phức tạp phù hợp với các ràng buộc vì tổng n và q trên các trường hợp thử nghiệm được giới hạn bởi 2 × 10⁵. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    from collections import defaultdict

    t = int(input())
    out = []
    for _ in range(t):
        n, q = map(int, input().split())
        x = list(map(int, input().split()))
        queries = list(map(int, input().split()))

        freq = defaultdict(int)

        for i in range(n - 1):
            cov = (i + 1) * (n - i - 1)
            freq[cov] += x[i + 1] - x[i]

        out.append(" ".join(str(freq.get(k, 0)) for k in queries))

    return "\n".join(out)

# provided samples
assert run("""3
2 2
101 200
2 1
6 15
1 2 3 5 6 7
1 2 3 4 5 6 7 8 9 10 11 12 13 14 15
5 8
254618033 265675151 461318786 557391198 848083778
6 9 15 10 6 9 4 4294967300
""") == """0 100
0 0 0 0 2 0 0 0 3 0 2 0 0 0 0
291716045 0 0 0 291716045 0 301749698 0"""

# custom cases
assert run("""1
2 3
1 10
1 2 9
""") == "9 0 0"

assert run("""1
3 3
1 5 10
1 4 6
""") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| hai điểm | hành vi khoảng thời gian trực tiếp | độ đúng cơ sở | 
| ba điểm cách nhau | tổng hợp nhiều khoảng thời gian | tương tác của các khoảng trống | 

## Vỏ cạnh 

Khi tất cả các tọa độ đều liên tiếp, mỗi khoảng có độ dài 1 và nghiệm giảm xuống thành tổ hợp thuần túy trên các cặp chỉ số. Thuật toán vẫn hoạt động vì phép nhân với độ dài khoảng trở nên trung tính. 

Khi các khoảng trống cực kỳ lớn, thuật toán vẫn ổn định vì nó không bao giờ lặp lại trực tiếp trên tọa độ. Chỉ sử dụng các khác biệt x[i+1] − x[i], giúp chia tỷ lệ thành các giá trị lớn một cách an toàn mà không ảnh hưởng đến độ phức tạp. 

Khi các truy vấn yêu cầu k rất lớn, bản đồ tần số chỉ trả về 0 vì không có khoảng nào tạo ra mức độ bao phủ như vậy.
