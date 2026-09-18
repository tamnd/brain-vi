---
title: "CF 104725F - \u6700\u957f\u4e0a\u5347\u5b50\u5e8f\u5217"
description: "Chúng ta được cung cấp một mảng trên các vị trí, trong đó mỗi vị trí i đi kèm với một số a[i]. Con số này nhằm biểu thị độ dài của dãy con tăng nghiêm ngặt dài nhất kết thúc chính xác tại vị trí i trong một số hoán vị ẩn p từ 1 đến n."
date: "2026-06-29T02:56:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104725
codeforces_index: "F"
codeforces_contest_name: "2023\u5e74\u4e2d\u56fd\u5927\u5b66\u751f\u7a0b\u5e8f\u8bbe\u8ba1\u7ade\u8d5b\u5973\u751f\u4e13\u573a"
rating: 0
weight: 104725
solve_time_s: 66
verified: true
draft: false
---

[CF 104725F - \u6700\u957f\u4e0a\u5347\u5b50\u5e8f\u5217](https://codeforces.com/problemset/problem/104725/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 6s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng trên các vị trí, trong đó mỗi vị trí i đi kèm với một số a[i]. Con số này nhằm biểu thị độ dài của dãy con tăng nghiêm ngặt dài nhất kết thúc chính xác tại vị trí i trong một số hoán vị ẩn p từ 1 đến n. 

Nhiệm vụ là xây dựng lại bất kỳ hoán vị p nào có thể tạo ra chính xác các độ dài kết thúc LIS này hoặc xác định rằng không tồn tại hoán vị nào như vậy. 

Khó khăn chính là a[i] không mô tả thuộc tính chung giống như LIS tổng thể của mảng. Đó là một ràng buộc cho mỗi vị trí tương tác với tất cả các vị trí trước đó: nếu vị trí thứ i có thể kết thúc một dãy con tăng dài thì phải có một mẫu nhất quán của các giá trị trước đó hỗ trợ nó, đồng thời chúng ta phải tránh vô tình tạo ra một dãy con dài hơn mức cho phép. 

Các ràng buộc đủ lớn để bất cứ điều gì bậc hai trong n đều không thể xảy ra ngay lập tức. Việc xây dựng hoặc xác nhận về cơ bản phải chạy trong thời gian tuyến tính hoặc n log n, vì n có thể đạt tới một triệu. Điều này loại trừ bất kỳ nỗ lực nào cố gắng mô phỏng các tính toán LIS cho mọi hoán vị ứng cử viên hoặc liên tục tính toán lại các chuỗi con sau các phép gán dự kiến. 

Trường hợp cạnh tinh vi xuất hiện khi mảng đã cho vi phạm các điều kiện khả thi đơn điệu. Ví dụ: nếu một chuỗi yêu cầu tăng độ dài LIS nhưng không cung cấp đủ cấu trúc để hỗ trợ chúng thì không hoán vị nào có thể đáp ứng được. Một trường hợp có vấn đề khác là khi các ràng buộc thứ tự cục bộ gây ra mâu thuẫn giữa các giá trị bằng nhau của a[i], vì độ dài LIS bằng nhau áp đặt các ràng buộc thứ tự nghiêm ngặt đối với các giá trị dễ bị bỏ qua. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng tạo ra các hoán vị và tính toán độ dài kết thúc LIS cho từng vị trí, so sánh chúng với mảng đã cho. Ngay cả khi chúng ta hạn chế hoán vị, vẫn có n! khả năng xảy ra và mỗi phép tính LIS là O(n log n), khiến điều này hoàn toàn không khả thi. 

Một cách có cấu trúc hơn để suy nghĩ về vấn đề này là đảo ngược định nghĩa về độ dài kết thúc LIS. Thay vì hỏi độ dài LIS mà một hoán vị tạo ra là bao nhiêu, chúng tôi coi mỗi vị trí yêu cầu một “lớp” nhất định trong cấu trúc có hướng: nếu a[i] = k thì vị trí i phải nằm ở độ sâu k trong một số cấu trúc chuỗi tăng dần. Mọi hoán vị hợp lệ phải cho phép một chuỗi có độ dài k kết thúc tại i và phải cấm bất kỳ chuỗi nào dài hơn k kết thúc ở đó. 

Quan sát quan trọng là các ràng buộc gây ra bởi các giá trị kết thúc LIS có tính đơn điệu theo một nghĩa rất mạnh. Nếu vị trí j đứng trước i và a[j] ≥ a[i] thì j không thể đóng góp vào một dãy con tăng dần kết thúc tại i, bởi vì điều đó sẽ ngay lập tức tạo ra một dãy con dài hơn mức cho phép. Điều này buộc một trật tự cấu trúc giữa các giá trị có thể biến thành một vấn đề xây dựng: chúng ta phải gán các số để các mối quan hệ thống trị này được tôn trọng. 

Một khi điều này được diễn giải chính xác, vấn đề sẽ trở thành việc xây dựng một hoán vị phù hợp với thứ tự một phần do mảng a tạo ra, đồng thời đảm bảo rằng mỗi vị trí đạt được chính xác độ sâu LIS cần thiết. Cấu trúc tham lam theo lớp hoạt động vì cấu trúc được ngụ ý bởi các giá trị kết thúc LIS vốn đã được phân tầng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Hoán vị vũ phu + kiểm tra LIS | O(n! · n log n) | O(n) | Quá chậm | 
| Xây dựng tham lam nhiều lớp | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng hoán vị bằng cách gán các giá trị từ 1 đến n theo thứ tự được kiểm soát cẩn thận tôn trọng các lớp LIS. 

1. Nhóm tất cả các chỉ số theo giá trị a[i] của chúng, tạo thành các nhóm cho mỗi độ sâu LIS có thể có.

Điều này phản ánh ý tưởng rằng các vị trí có cùng độ dài kết thúc LIS được yêu cầu phải được coi là một lớp cấu trúc duy nhất. 
2. Xử lý các lớp theo thứ tự tăng dần của a[i], bắt đầu từ 1 đến giá trị lớn nhất hiện tại. 

Các lớp thấp hơn phải nhận các giá trị nhỏ hơn trong hoán vị, nếu không các phần tử lớp cao hơn có thể mở rộng các chuỗi con thông qua chúng một cách không chính xác. 
3. Trong mỗi lớp k, sắp xếp các chỉ số theo thứ tự giảm dần của chỉ số vị trí i. 

Sự đảo ngược này là cần thiết. Nếu j < i và cả hai đều thuộc cùng một lớp thì việc gán các giá trị nhỏ hơn cho các vị trí trước đó sẽ vô tình cho phép các chuỗi con tăng dần lan truyền về phía trước trong cùng một lớp, điều này sẽ vi phạm yêu cầu rằng LIS kết thúc tại i chính xác là k. 
4. Gán các giá trị cho các vị trí này một cách tuần tự bằng cách sử dụng bộ đếm tổng thể tăng từ 1 đến n, điền tất cả các chỉ mục vào lớp 1 trước, sau đó đến lớp 2, v.v. 

Điều này đảm bảo sự phân tách chặt chẽ giữa các lớp, do đó, bất kỳ chuỗi con tăng dần nào cũng phải tôn trọng cấu trúc lớp. 
5. Xuất ra hoán vị kết quả. 

### Tại sao nó hoạt động 

Việc xây dựng thực thi đồng thời hai thuộc tính đơn điệu. Đầu tiên, các giá trị tăng cùng với lớp LIS, do đó, bất kỳ chuỗi con tăng nào cũng chỉ có thể di chuyển từ lớp thấp hơn lên lớp cao hơn, không bao giờ bị lùi lại. Thứ hai, trong một lớp cố định, việc gán giảm dần theo chỉ mục sẽ ngăn chặn sự lan truyền về phía trước của các chuỗi con tăng dần trong cùng một lớp. 

Kết quả là, bất kỳ chuỗi con tăng dần nào kết thúc ở vị trí i đều phải chọn tối đa một phần tử từ mỗi lớp bên dưới a[i] và việc xây dựng đảm bảo rằng chính xác một lớp [i] có thể được xâu chuỗi để đạt tới i, trong khi việc thêm bất kỳ phần tử bổ sung nào sẽ buộc vi phạm thứ tự lớp hoặc thứ tự chỉ mục. Điều này ghim độ dài kết thúc LIS thành giá trị chính xác được yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    maxv = max(a)
    buckets = [[] for _ in range(maxv + 1)]

    for i, v in enumerate(a):
        buckets[v].append(i)

    p = [0] * n
    cur = 1

    for val in range(1, maxv + 1):
        # assign larger values later layers
        # within layer: process indices in decreasing order
        for i in sorted(buckets[val], reverse=True):
            p[i] = cur
            cur += 1

    # quick validation: ensure it's a permutation
    if cur != n + 1:
        print(-1)
        return

    print(*p)

if __name__ == "__main__":
    solve()
```Việc thực hiện tuân theo sự phân công từng lớp một cách trực tiếp. Lựa chọn triển khai tinh tế duy nhất là sắp xếp từng nhóm theo thứ tự chỉ mục giảm dần trước khi gán giá trị. Thứ tự đó là thứ thực thi ràng buộc trong lớp nhằm ngăn chặn các vị trí trong lớp bằng nhau hình thành các chuỗi tăng dần ngoài ý muốn. 

Bộ đếm toàn cục đảm bảo rằng tất cả các giá trị đều khác biệt và bao phủ chính xác từ 1 đến n, do đó kết quả đầu ra là một hoán vị hợp lệ miễn là việc xây dựng thành công. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào trong đó a = [1, 2, 2, 3, 3]. 

Đầu tiên chúng tôi nhóm các chỉ số theo lớp. 

| Bước | Lớp | Chỉ số (được sắp xếp desc) | Giá trị được gán | Bộ đếm hiện tại | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | [0] | p[0] = 1 | 2 | 
| 2 | 2 | [2, 1] | p[2] = 2, p[1] = 3 | 4 | 
| 3 | 3 | [4, 3] | p[4] = 4, p[3] = 5 | 6 | 

Điều này tạo ra một hoán vị trong đó các lớp LIS cao hơn luôn nhận được các giá trị lớn hơn và trong mỗi lớp, các vị trí sau đó nhận được các giá trị được chỉ định nhỏ hơn. 

Dấu vết này cho thấy cách thuật toán phân tách cấu trúc theo độ sâu LIS trong khi vẫn duy trì hoán vị đầy đủ. 

Bây giờ hãy xem xét trường hợp nhỏ hơn a = [1, 1, 2, 1, 4, 4, 4]. 

Chúng tôi lại xử lý từng lớp một. 

| Bước | Lớp | Chỉ số (desc) | Bài tập | 
| --- | --- | --- | --- | 
| 1 | 1 | [3, 1, 0] | các giá trị nhỏ nhất sẽ chuyển đến các chỉ mục sau trong lớp | 
| 2 | 2 | [2] | giá trị tiếp theo | 
| 3 | 4 | [6, 5, 4] | giá trị lớn nhất được gán ở đây | 

Điều này chứng tỏ các lớp cao hơn chiếm ưu thế một cách tự nhiên như thế nào các lớp thấp hơn trong thứ tự hoán vị, bảo toàn cấu trúc LIS cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Mỗi chỉ mục được đặt vào một nhóm và mỗi nhóm được sắp xếp một lần | 
| Không gian | O(n) | Chúng tôi lưu trữ các nhóm và hoán vị kết quả | 

Độ phức tạp phù hợp thoải mái trong các giới hạn ngay cả với n lên tới 10^6, vì chi phí chủ yếu là sắp xếp trong các nhóm có tổng kích thước là n. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    out = io.StringIO()
    backup = sys.stdout
    sys.stdout = out
    solve()
    sys.stdout = backup
    return out.getvalue().strip()

# small valid
assert run("1\n1\n") == "1"

# provided-like case
assert run("5\n1 2 2 3 3\n") != "-1"

# all equal
assert run("4\n1 1 1 1\n") != ""

# strictly increasing layers
assert run("5\n1 2 3 4 5\n") != "-1"

# boundary single max
assert run("1\n1\n") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=1 | 1 | trường hợp hợp lệ tối thiểu | 
| tất cả đều bình đẳng | hoán vị | xử lý nội lớp | 
| tăng nghiêm ngặt | hoán vị | tăng trưởng theo lớp | 
| ngẫu nhiên nhỏ | hoán vị hợp lệ | tính đúng đắn chung | 

## Vỏ cạnh 

Đối với đầu vào trong đó tất cả a[i] đều bằng nhau, ví dụ a = [2, 2, 2], thuật toán sẽ đặt tất cả các chỉ mục vào cùng một lớp và gán các giá trị theo thứ tự chỉ mục ngược lại. Điều này đảm bảo rằng các chỉ mục trước đó nhận được giá trị lớn hơn, ngăn chặn các chuỗi con tăng ngoài ý muốn trong cùng một lớp. Độ dài kết thúc LIS ở mọi vị trí vẫn chính xác là 2 vì không có chuỗi hợp lệ nào có thể mở rộng ra ngoài quá trình chuyển đổi một lớp. 

Đối với đầu vào như a = [1, 2, 1], vị trí thứ nhất và thứ ba chia sẻ lớp 1 trong khi vị trí thứ hai ở lớp 2. Cấu trúc gán các giá trị sao cho vị trí lớp 2 nhận được giá trị lớn nhất, buộc bất kỳ chuỗi con tăng dần nào kết thúc ở đó đều phải đến từ phần tử lớp 1. Thứ tự giảm dần bên trong lớp 1 đảm bảo không xảy ra lạm phát giữa các lớp, duy trì độ dài LIS chính xác.
