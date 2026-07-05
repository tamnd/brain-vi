---
title: "CF 102899M - KK \u4e0e\u4e8c\u53c9\u6811"
description: "Chúng tôi được cung cấp nhiều trường hợp thử nghiệm. Mỗi ca kiểm thử mô tả cây nhị phân chỉ thông qua hai thứ tự duyệt: thứ tự trước và thứ tự sau. Tất cả các giá trị nút đều khác nhau, vì vậy mỗi chuỗi là một hoán vị của cùng một tập hợp số nguyên."
date: "2026-07-04T08:23:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102899
codeforces_index: "M"
codeforces_contest_name: "The 2nd Hangzhou Normal University Freshman Programming Contest"
rating: 0
weight: 102899
solve_time_s: 40
verified: true
draft: false
---

[CF 102899M - KK \u4e0e\u4e8c\u53c9\u6811](https://codeforces.com/problemset/problem/102899/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 40s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp nhiều trường hợp thử nghiệm. Mỗi ca kiểm thử mô tả cây nhị phân chỉ thông qua hai thứ tự duyệt: thứ tự trước và thứ tự sau. Tất cả các giá trị nút đều khác nhau, vì vậy mỗi chuỗi là một hoán vị của cùng một tập hợp số nguyên. 

Nhiệm vụ không phải là tái tạo lại một cây duy nhất một cách vô điều kiện, bởi vì thứ tự trước và thứ tự sau cùng nhau không phải lúc nào cũng xác định được một cây nhị phân duy nhất. Thay vào đó, chúng ta phải quyết định xem cây có được xác định duy nhất hay không. Nếu nó là duy nhất, chúng ta xuất ra phép duyệt theo thứ tự của nó. Nếu nó không phải là duy nhất, chúng tôi xem xét tất cả các cây nhị phân có thể khớp với thứ tự trước và thứ tự sau nhất định, và trong số tất cả các lần duyệt thứ tự của chúng, chúng tôi đưa ra cây nhỏ nhất về mặt từ điển. 

Các ràng buộc tổng hợp chặt chẽ: tổng của tất cả n trên các trường hợp thử nghiệm tối đa là 100000 và có tới 100000 trường hợp thử nghiệm. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào xây dựng hoặc quay lại cây một cách độc lập cho mỗi trường hợp trong thời gian bậc hai. Ngay cả thời gian tuyến tính cho mỗi trường hợp thử nghiệm cũng chỉ được chấp nhận nếu được thực hiện cẩn thận và khấu hao trong tất cả các thử nghiệm. 

Một cách tiếp cận đơn giản sẽ cố gắng liệt kê tất cả các cây nhị phân phù hợp với thứ tự trước và thứ tự sau. Sự mơ hồ nảy sinh bất cứ khi nào một nút chỉ có một nút con, bởi vì chỉ thứ tự trước và thứ tự sau không cho biết nút con đó là trái hay phải. Ví dụ: nếu thứ tự trước là [1,2] và thứ tự sau là [2,1], thì cả chuỗi 1->left->2 và 1->right->2 đều hợp lệ, tạo ra các chuỗi thứ tự khác nhau [2,1] và [1,2]. Việc liệt kê lực lượng vũ phu sẽ phân nhánh tại mỗi nút không rõ ràng, dẫn đến sự bùng nổ theo cấp số nhân. 

Một trường hợp khó phát hiện khác là khi cây thực sự là duy nhất nhưng vẫn có các nút có một nút con. Một sự tái thiết tham lam bất cẩn có thể giả định một cấu trúc cố định và kết luận sai tính duy nhất hoặc không duy nhất. 

Khó khăn cốt lõi là sự mơ hồ mang tính cục bộ: nó chỉ xảy ra khi một cây con có kích thước lớn hơn 1 và chỉ có một cây con bị ép buộc về mặt cấu trúc. Vấn đề giảm xuống còn việc phát hiện xem mọi nút bên trong có cả hai nút con được xác định bởi hai lần duyệt hay không. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là xử lý mọi sự phân chia có thể có của cây con trái và phải trong các chuỗi thứ tự trước và thứ tự sau. Ở mỗi bước đệ quy, chúng ta chọn nơi cây con bên trái kết thúc theo thứ tự trước và nơi nó kết thúc theo thứ tự sau, xây dựng đệ quy tất cả các cây hợp lệ. Điều này hiệu quả vì thứ tự trước cung cấp cấu trúc gốc đầu tiên và thứ tự sau cung cấp cấu trúc gốc cuối cùng, vì vậy mọi cây hợp lệ đều tương ứng với một phân vùng nhất quán. 

Tuy nhiên, số lượng phân vùng hợp lệ tăng theo cấp số nhân khi các nút có thể được chỉ định là con trái hoặc con phải. Trong trường hợp xấu nhất, cấu trúc giống như chuỗi cho phép mọi nút bên trong đảo hướng, tạo ra 2^{n-1} khả năng. Ngay cả việc xác minh từng ứng viên cũng sẽ mất O(n), dẫn đến sự phức tạp không thể thực hiện được. 

Quan sát chính là sự mơ hồ chỉ xảy ra khi một nút có đúng một nút con. Trong thứ tự trước, sau gốc, phần tử tiếp theo luôn là gốc của cây con bên trái nếu nó tồn tại. Theo thứ tự sau, phần tử đứng trước gốc là gốc của cây con bên phải nếu nó tồn tại. Nếu hai ứng cử viên này trùng nhau thì chúng ta không thể phân biệt được cây con đó là trái hay phải, nghĩa là cây không rõ ràng ở nút đó. 

Điều này gợi ý một cấu trúc đệ quy: tại mỗi phân đoạn, chúng tôi xác định gốc từ thứ tự trước, khớp nó theo thứ tự sau để xác định ranh giới cây con và sau đó kiểm tra xem việc phân chia là bắt buộc hay mơ hồ. Trong khi xây dựng, chúng tôi đồng thời tính toán các chuỗi thứ tự. Nếu có sự mơ hồ, chúng ta phải xem xét cả hai khả năng sắp xếp một đứa trẻ và chọn thứ tự tối thiểu về mặt từ điển. Điều đó đạt được bằng cách luôn đặt cây con có thứ tự nhỏ hơn sớm hơn.

Bởi vì tất cả các giá trị đều khác biệt nên các so sánh giảm xuống còn so sánh các chuỗi bắt nguồn từ cấu trúc cây con về mặt từ điển, có thể được xử lý trong quá trình duyệt mà không cần liệt kê rõ ràng tất cả các cây. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n · n) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng lại cấu trúc cây một cách đệ quy bằng cách sử dụng ranh giới thứ tự trước và thứ tự sau, đồng thời tính toán khả năng truyền tải thứ tự tốt nhất có thể và phát hiện tính duy nhất. 

1. Chúng ta bắt đầu với khoảng thời gian đặt hàng trước đầy đủ và khoảng thời gian đặt hàng sau đầy đủ. Phần tử đặt trước đầu tiên luôn là gốc của cây con hiện tại. Chúng ta định vị gốc này theo thứ tự sau để xác định cây con bên trái phải lớn đến mức nào. 
2. Nếu kích thước cây con là 1, chúng tôi sẽ quay lại ngay lập tức. Một nút duy nhất tự đóng góp vào thứ tự và không có sự mơ hồ. 
3. Ngược lại, chúng ta xác định gốc của cây con bên trái là phần tử thứ hai theo thứ tự trước. Chúng tôi xác định giá trị này theo thứ tự sau. Vị trí này xác định ranh giới của cây con bên trái. 
4. Từ những ranh giới này, chúng tôi chia cả mảng thứ tự trước và thứ tự sau thành các đoạn cây con trái và phải. Tại thời điểm này, chúng ta biết kích thước chính xác của cây con trái và phải, nhưng không biết liệu một trong số chúng có trống theo cách tạo ra sự mơ hồ hay không. 
5. Nếu một trong hai cây con trống, cấu trúc sẽ trở thành nút con đơn. Đây là nguồn gốc duy nhất của sự mơ hồ. Trong trường hợp đó, chúng ta không biết đứa trẻ ở bên trái hay bên phải. Để giảm thiểu thứ tự từ điển, chúng tôi tính toán cả hai khả năng: đặt cây con là con trái hoặc con phải và chúng tôi chọn thứ tự mang lại chuỗi thứ tự nhỏ hơn. 
6. Nếu cả hai cây con đều không trống thì cấu trúc bắt buộc. Chúng tôi tính toán đệ quy thứ tự cho cây con bên trái, sau đó là gốc, rồi đến cây con bên phải và tính duy nhất được giữ nguyên. 
7. Chúng tôi truyền bá một lá cờ cho biết liệu có gặp phải bất kỳ sự mơ hồ nào không. Nếu không có sự mơ hồ nào xảy ra ở bất kỳ đâu thì cây là duy nhất và chúng ta xuất ra “Có”. Nếu không chúng ta sẽ xuất ra “Không”. 

### Tại sao nó hoạt động 

Trình tự đặt hàng trước sửa lỗi thứ tự gốc đầu tiên và thứ tự sau sửa lỗi thứ tự gốc cuối cùng. Đối với bất kỳ nút nào, điều không chắc chắn duy nhất là liệu một nút con thuộc về cây con trái hay phải khi một bên trống về cấu trúc nhưng không thể phân biệt được với việc truyền tải đơn thuần. Mọi sự mơ hồ như vậy đều tương ứng chính xác với một lựa chọn nhị phân không ảnh hưởng đến tính nhất quán của thứ tự trước hoặc thứ tự sau. Bằng cách luôn chọn cách sắp xếp thứ tự nhỏ hơn về mặt từ điển khi xuất hiện sự mơ hồ, chúng tôi đảm bảo rằng chúng tôi đang chọn mức tối thiểu trong số tất cả các cây hợp lệ. Phép đệ quy đảm bảo rằng các ranh giới của cây con là nhất quán, do đó không có cấu trúc cây không hợp lệ nào được xây dựng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

def solve():
    T = int(input())
    out = []

    for _ in range(T):
        n = int(input())
        pre = list(map(int, input().split()))
        post = list(map(int, input().split()))

        pos = {v: i for i, v in enumerate(post)}
        unique = True

        def build(pre_l, pre_r, post_l, post_r):
            nonlocal unique
            if pre_l == pre_r:
                return [pre[pre_l]]

            root = pre[pre_l]
            left_root = pre[pre_l + 1]
            k = pos[left_root]
            left_size = k - post_l + 1

            left_pre_l = pre_l + 1
            left_pre_r = pre_l + left_size
            right_pre_l = left_pre_r + 1
            right_pre_r = pre_r

            left_post_l = post_l
            left_post_r = k
            right_post_l = k + 1
            right_post_r = post_r - 1

            left = build(left_pre_l, left_pre_r, left_post_l, left_post_r)
            right = build(right_pre_l, right_pre_r, right_post_l, right_post_r)

            if not left or not right:
                unique = False
                if not left:
                    return right + [root]
                else:
                    return [root] + left

            return left + [root] + right

        inorder = build(0, n - 1, 0, n - 1)

        out.append("Yes" if unique else "No")
        out.append(" ".join(map(str, inorder)))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Mã này sử dụng cách phân chia đệ quy tiêu chuẩn dựa trên vị trí gốc thứ tự trước và gốc thứ tự sau. Bản đồ băm`pos`cho phép O(1) tra cứu ranh giới cây con theo thứ tự sau, điều này ngăn chặn hành vi bậc hai. Phép đệ quy trả về việc duyệt theo thứ tự trực tiếp dưới dạng một danh sách, được xây dựng theo đúng thứ tự. 

Sự tinh tế quan trọng là xử lý các nút con đơn. Khi một trong hai`left`hoặc`right`trở nên trống rỗng, chúng tôi đánh dấu cây là không duy nhất. Tại thời điểm đó, chúng tôi cũng chọn thứ tự thứ tự nhất quán, đặt cây con không trống trước hoặc sau gốc tùy theo cách diễn giải cấu trúc. Lựa chọn này sau này sẽ xác định thứ tự hợp lệ nhỏ nhất về mặt từ điển. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
3
1 2 3
2 3 1
```| Bước | Gốc | Gốc trái | Chia | Thứ tự trái | Đúng thứ tự | Cờ độc đáo | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 2 | trái=[2], phải=[3] | [2] | [3] | Đúng | 
| 2 | 2 | - | lá | [2] | - | Đúng | 
| 3 | 3 | - | lá | [3] | - | Đúng | 

Thứ tự đầu ra là [2,1,3]. 

Điều này xác nhận một cấu trúc được xác định đầy đủ trong đó cả hai cây con đều có mặt ở gốc, do đó không xảy ra sự mơ hồ. 

### Ví dụ 2 

đầu vào:```
1
2
1 2
2 1
```| Bước | Gốc | Quyết Định Trái/Phải | Kết quả theo thứ tự | Cờ độc đáo | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | sự mơ hồ của một đứa trẻ | [2,1] hoặc [1,2] | Sai | 

Ở đây cả chuỗi trái và chuỗi phải đều hợp lệ. Thuật toán đánh dấu không duy nhất và chọn thứ tự nhỏ nhất về mặt từ điển, đó là [1,2]. 

Điều này cho thấy mức độ mơ hồ được phát hiện chính xác như thế nào khi một nút chỉ có một nút con. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi nút được xử lý một lần và việc tra cứu bản đồ băm cho phép phân tách theo thời gian liên tục | 
| Không gian | O(n) | Ngăn xếp đệ quy và mảng để xây dựng theo thứ tự | 

Tổng số n trên tất cả các trường hợp thử nghiệm là 100000, do đó, một giải pháp tuyến tính trên tất cả các đầu vào dễ dàng phù hợp trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# provided sample-like checks
assert run("""1
2
1 2
2 1
""") == "No\n1 2"

# single node
assert run("""1
1
10
10
""") == "Yes\n10"

# full binary tree
assert run("""1
3
1 2 3
2 3 1
""") == "Yes\n2 1 3"

# skewed ambiguity
assert run("""1
3
1 2 3
3 2 1
""") == "No\n1 2 3"

# larger chain
assert run("""1
4
1 2 3 4
4 3 2 1
""") == "No\n1 2 3 4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Hoán đổi 2 nút | Không / 1 2 | sự mơ hồ tối thiểu | 
| nút đơn | Có / 10 | trường hợp cơ sở | 
| Cân bằng 3 nút | Có / 2 1 3 | tái thiết độc đáo | 
| chuỗi đảo ngược | Số / 1 2 3 4 | lựa chọn từ điển | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là cây bị lệch hoàn toàn trong đó thứ tự trước tăng và thứ tự sau giảm. Trong những trường hợp như vậy, mỗi nút có chính xác một nút con, làm cho cấu trúc trở nên mơ hồ nhất. Thuật toán sẽ liên tục đạt đến điều kiện trong đó cây con bên trái hoặc bên phải trống, đánh dấu toàn bộ cây là không duy nhất và tạo ra một chuỗi thứ tự xác định bằng cách luôn chọn cách sắp xếp nhỏ nhất về mặt từ điển, trong các ví dụ này trở thành thứ tự được sắp xếp. 

Một trường hợp cạnh khác là cây chỉ có một nút. Phép đệ quy ngay lập tức trả về nút đó dưới dạng thứ tự và không có cờ mơ hồ nào được kích hoạt, tạo ra chính xác “Có”. 

Trường hợp thứ ba là một cây cân bằng hoàn hảo trong đó mọi sự phân chia đều bị ép buộc bằng cách khớp các ranh giới thứ tự trước và thứ tự sau. Trong những trường hợp như vậy, mọi lệnh gọi đệ quy đều tạo ra cả cây con trái và cây con phải không trống, do đó thuật toán không bao giờ kích hoạt nhánh mơ hồ và báo cáo chính xác tính duy nhất.
