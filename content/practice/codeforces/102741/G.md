---
title: "CF 102741G - Thư giữa chúng ta"
description: "Chúng ta được cấp một bảng hình chữ nhật gồm các chữ cái viết thường. Mỗi hàng là một chuỗi và chúng ta được phép xóa toàn bộ các cột khỏi bảng."
date: "2026-07-29T00:46:31+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102741
codeforces_index: "G"
codeforces_contest_name: "UTPC Contest 9-25-20 Div. 1"
rating: 0
weight: 102741
solve_time_s: 58
verified: true
draft: false
---

[CF 102741G - Thư giữa chúng ta](https://codeforces.com/problemset/problem/102741/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 58s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một bảng hình chữ nhật gồm các chữ cái viết thường. Mỗi hàng là một chuỗi và chúng ta được phép xóa toàn bộ các cột khỏi bảng. Mục đích là loại bỏ càng ít cột càng tốt để sau khi các cột còn lại được nối với nhau, các hàng sẽ xuất hiện theo thứ tự từ điển không giảm dần từ trên xuống dưới. Tác vụ ban đầu yêu cầu số lượng cột tối thiểu phải biến mất. 

Chi tiết quan trọng là việc xóa một cột sẽ ảnh hưởng đến mọi hàng ở cùng một vị trí. Chúng tôi không được phép sắp xếp lại các hàng hoặc chỉnh sửa các ký tự riêng lẻ. Một cột trông không hợp lý đối với một cặp hàng lân cận nhưng vẫn có thể hữu ích cho các hàng khác, do đó, quyết định phải xem xét tất cả các so sánh hàng liền kề với nhau. 

Các ràng buộc xác định các chiến lược có thể. Nếu lưới có khoảng$10^5$các ô, chúng ta có thể đủ khả năng để kiểm tra từng ký tự với số lần không đổi. Bất kỳ giải pháp nào thử mọi tập hợp con của cột đều không thể thực hiện được vì số lượng tập hợp con là theo cấp số nhân. Ngay cả việc thử tất cả các thứ tự cột có thể có hoặc liên tục xây dựng lại lưới sau mỗi lần xóa cũng sẽ quá chậm. Giải pháp cần đưa ra quyết định cục bộ trong khi quét lưới một lần. 

Một số trường hợp nguy hiểm có thể phá vỡ việc triển khai bất cẩn. Nếu tất cả các hàng đã được sắp xếp thì không được xóa cột nào. 

Ví dụ đầu vào:```
3 3
abc
abd
acd
```Đầu ra đúng là:```
0
```Giải pháp xóa mọi cột chứa mức giảm ở đâu đó sẽ loại bỏ các cột không cần thiết vì cột đầu tiên đã phân tách các hàng một cách chính xác. 

Một trường hợp phức tạp khác là khi hai hàng được sắp xếp trước một cột sau. 

Ví dụ đầu vào:```
3 3
abc
bbc
bba
```Đầu ra đúng là:```
1
```Cột đầu tiên chứng tỏ hàng đầu tiên nhỏ hơn hàng thứ hai và thứ ba. Chỉ còn lại sự so sánh giữa hàng thứ hai và thứ ba, còn cột cuối cùng tạo ra xung đột. Giải pháp liên tục kiểm tra các cặp hàng đã được giải quyết có thể xóa nhầm nhiều cột hơn. 

Trường hợp cạnh cuối cùng là khi tồn tại các hàng bằng nhau. 

Ví dụ đầu vào:```
2 3
aaa
aaa
```Đầu ra đúng là:```
0
```Các hàng bằng nhau đã hợp lệ vì thứ tự được yêu cầu không giảm, không tăng hoàn toàn. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là thử loại bỏ các cột và kiểm tra xem lưới còn lại có được sắp xếp hay không. Đối với mọi tập hợp cột bị loại bỏ có thể, chúng tôi có thể tạo các hàng kết quả và so sánh các hàng liền kề theo từ điển. Điều này đúng vì nó kiểm tra chính xác điều kiện mà bài toán yêu cầu. Tuy nhiên, với$m$cột có$2^m$những lựa chọn có thể xảy ra, do đó số lượng kiểm tra tăng theo cấp số nhân. Ngay cả với lưới điện nhỏ, điều này nhanh chóng trở thành không thể. 

Một nỗ lực tốt hơn một chút là kiểm tra từng cột một và xóa bất kỳ cột nào trong đó một số cặp liền kề có ký tự lớn hơn ký tự nhỏ hơn. Vấn đề là không phải sự so sánh nào cũng cần được cân nhắc mãi mãi. Khi một cột được giữ trước đó chứng minh hàng đó$i$nhỏ hơn hàng$i+1$, các cột trong tương lai không thể thay đổi mối quan hệ đó. Tiếp tục coi cặp đó là nguồn xung đột có thể dẫn đến việc xóa không cần thiết. 

Quan sát quan trọng là so sánh từ điển chỉ phụ thuộc vào cột đầu tiên nơi hai hàng khác nhau. Khi quét các cột từ trái sang phải, chúng ta có thể nhớ những cặp hàng liền kề nào đã được các cột trước đó quyết định. Đối với cột hiện tại, xung đột chỉ tồn tại giữa các cặp chưa được giải quyết. Nếu cột hiện tại làm cho bất kỳ cặp chưa được giải quyết nào giảm đi thì cột đó không thể được giữ lại. Mặt khác, việc giữ nó luôn an toàn và mọi cặp chưa được giải quyết trở nên tăng nghiêm ngặt đều có thể được đánh dấu là đã giải quyết. 

Lực lượng vũ phu hoạt động vì nó liên tục hỏi liệu lựa chọn cột hiện tại có tạo ra thứ tự hợp lệ hay không, nhưng không thành công vì nó bỏ qua thực tế là nhiều so sánh hàng sẽ được giải quyết vĩnh viễn. Việc quan sát các cặp liền kề đã được giải quyết giúp giảm vấn đề xuống còn một lần quét tham lam. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^m \cdot nm)$|$O(nm)$| Quá chậm | 
| Tối ưu |$O(nm)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc lưới và tạo một mảng biểu thị xem mỗi cặp hàng liền kề đã được xác định theo đúng thứ tự hay chưa. 

Một cặp hàng lân cận chỉ cần được chú ý cho đến khi một số cột được giữ lại chứng tỏ hàng trên nhỏ hơn. Sau thời điểm đó, các cột sau không thể làm cho cặp này không hợp lệ. 
2. Quét các cột từ trái sang phải. 

Các cột bên trái có mức độ ưu tiên cao hơn trong việc so sánh từ điển, do đó, quyết định về một cột phải được đưa ra trước khi xem các cột sau. 
3. Đối với cột hiện tại, hãy kiểm tra từng cặp hàng liền kề chưa được giải quyết. 

Nếu ký tự ở hàng trên lớn hơn ký tự bên dưới thì việc giữ nguyên cột này sẽ khiến cho việc sắp xếp cuối cùng không thể thực hiện được. Cột phải được loại bỏ. 
4. Nếu cột không tạo ra xung đột, hãy giữ nó và cập nhật các cặp đã giải quyết. 

Bất cứ khi nào một cặp chưa được giải quyết có ký tự ở hàng trên nhỏ hơn ở hàng dưới, cặp đó hiện được sắp xếp vĩnh viễn và không cần kiểm tra trong tương lai. 
5. Đếm từng cột bị từ chối và kết quả đầu ra được tính. 

Các cột bị từ chối chính xác là các cột không thể tham gia vào bất kỳ giải pháp hợp lệ nào. 

Tại sao nó hoạt động: 

Thuật toán duy trì tính bất biến rằng mọi cặp liền kề được giải quyết đều đã được sắp xếp chính xác bằng cách chỉ sử dụng các cột được giữ lại đã được xử lý cho đến nay. Đối với một cặp chưa được giải quyết, tất cả các cột được giữ trước đó đều chứa các ký tự bằng nhau, vì vậy cột hiện tại là vị trí đầu tiên có thể quyết định cặp này. Nếu nó giảm, không có cột nào trong tương lai có thể sửa lại thứ tự vì thứ tự từ điển đã được xác định không chính xác. Nếu nó tăng lên, cặp tiền sẽ có giá trị vĩnh viễn. Vì mỗi cột được giữ lại bất cứ khi nào nó không vi phạm bất kỳ so sánh chưa được giải quyết nào, thuật toán chỉ loại bỏ các cột buộc phải loại bỏ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    grid = [input().strip() for _ in range(n)]

    resolved = [False] * (n - 1)
    answer = 0

    for col in range(m):
        bad = False

        for row in range(n - 1):
            if not resolved[row] and grid[row][col] > grid[row + 1][col]:
                bad = True
                break

        if bad:
            answer += 1
            continue

        for row in range(n - 1):
            if not resolved[row] and grid[row][col] < grid[row + 1][col]:
                resolved[row] = True

    print(answer)

if __name__ == "__main__":
    solve()
```các`resolved`mảng lưu trữ trạng thái của mỗi so sánh hàng lân cận. chiều dài của nó là`n - 1`vì các hàng chỉ cần so sánh với hàng ngay bên dưới chúng. 

Đối với mỗi cột, mã đầu tiên sẽ kiểm tra sự mâu thuẫn. Điều này phải xảy ra trước khi cập nhật`resolved`, vì cột bị xóa không thể ảnh hưởng đến thứ tự. Nếu không có mâu thuẫn nào tồn tại, vòng lặp thứ hai đánh dấu những so sánh mới được giải quyết. 

Việc triển khai không bao giờ xây dựng lưới còn lại, điều này tránh được việc sao chép không cần thiết. Nó chỉ đọc mỗi ô một số lần không đổi, phù hợp với yêu cầu quét tuyến tính trên lưới. Điều kiện biên`n - 1`cũng quan trọng vì không có cặp liền kề nào sau hàng cuối cùng. 

## Ví dụ đã hoạt động 

Hãy xem xét:```
3 3
abc
abd
acd
```| Cột | Đã giải quyết các cặp trước | Hành động | Đã giải quyết các cặp sau | Trả lời | 
| --- | --- | --- | --- | --- | 
| 0 | [Sai, Sai] | Giữ,`a<a`giải quyết cặp đầu tiên | [Đúng, Sai] | 0 | 
| 1 | [Đúng, Sai] | Giữ,`b<c`giải quyết cặp thứ hai | [Đúng, Đúng] | 0 | 
| 2 | [Đúng, Đúng] | Giữ | [Đúng, Đúng] | 0 | 

Dấu vết cho thấy rằng khi một cặp được giải quyết, các cột sau đó sẽ bị bỏ qua để so sánh. Thứ tự cuối cùng đã hợp lệ nên không có cột nào bị xóa. 

Bây giờ hãy xem xét:```
3 3
abc
bbc
bba
```| Cột | Đã giải quyết các cặp trước | Hành động | Đã giải quyết các cặp sau | Trả lời | 
| --- | --- | --- | --- | --- | 
| 0 | [Sai, Sai] | Giữ, cặp đầu tiên giải quyết | [Đúng, Sai] | 0 | 
| 1 | [Đúng, Sai] | Giữ, cặp thứ hai chưa được giải quyết vì`b=b`| [Đúng, Sai] | 0 | 
| 2 | [Đúng, Sai] | Mâu thuẫn vì`c>a`cho cặp thứ hai | [Đúng, Sai] | 1 | 

Hàng thứ hai và thứ ba chỉ được so sánh bằng cột thứ ba vì hai ký tự đầu tiên của chúng bằng nhau. Cột đó phải được loại bỏ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(nm)$| Mỗi cột sẽ kiểm tra và có thể cập nhật tất cả các cặp hàng liền kề. | 
| Không gian |$O(n)$| Chỉ có lưới và trạng thái phân giải được lưu trữ, với bộ nhớ làm việc bổ sung tỷ lệ thuận với số lượng hàng. | 

Thuật toán thực hiện một lượng nhỏ công việc cho mỗi ô trong lưới, do đó nó phù hợp với các ràng buộc đã định. Bộ nhớ bổ sung được sử dụng để theo dõi các mối quan hệ hàng là tuyến tính theo số lượng hàng. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    output = sys.stdout.getvalue()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return output

# Sample-like cases
assert run("""3 3
abc
abd
acd
""") == "0\n", "already sorted"

assert run("""3 3
abc
bbc
bba
""") == "1\n", "late conflict"

# Minimum size
assert run("""2 1
a
a
""") == "0\n", "equal single characters"

# All equal values
assert run("""4 5
aaaaa
aaaaa
aaaaa
aaaaa
""") == "0\n", "all rows equal"

# Boundary where every column is bad
assert run("""3 3
cba
bca
abc
""") == "2\n", "multiple deletions"

# Add the actual solver here if testing separately
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Hàng được sắp xếp | 0 | Cột không cần xóa khi đã có đơn hàng | 
| Xung đột muộn | 1 | Các cặp đã giải quyết phải được bỏ qua sau | 
| Cột đơn | 0 | Kích thước tối thiểu và xử lý bình đẳng | 
| Hàng bằng nhau | 0 | Không giảm cho phép trùng lặp | 
| Nhiều cột xấu | 2 | Đếm đúng số lần xóa bắt buộc | 

## Vỏ cạnh 

Đối với trường hợp đã được sắp xếp:```
3 3
abc
abd
acd
```Cột đầu tiên giải quyết so sánh đầu tiên và cột thứ hai giải quyết so sánh thứ hai. Cột cuối cùng không bao giờ có thể tạo ra xung đột vì cả hai cặp đều đã được quyết định. Đầu ra của thuật toán`0`. 

Đối với trường hợp một cặp được giải quyết trước một cặp khác:```
3 3
abc
bbc
bba
```Cột đầu tiên cố định vĩnh viễn mối quan hệ giữa hàng thứ nhất và hàng thứ hai. Hàng thứ hai và thứ ba vẫn chưa được giải quyết cho đến cột cuối cùng, nơi thứ tự của chúng trở nên không chính xác. Thuật toán chỉ xóa cột đó và đưa ra kết quả`1`. 

Đối với các hàng bằng nhau:```
2 3
aaa
aaa
```Mọi so sánh vẫn chưa được giải quyết vì không có cột nào tạo ra thứ tự nghiêm ngặt nhưng cũng không có cột nào tạo ra xung đột. Thuật toán giữ tất cả các cột và kết quả đầu ra`0`, xử lý chính xác các hàng bằng nhau là hợp lệ. 

Đối với nhiều lần xóa bắt buộc:```
3 3
cba
bca
abc
```Quá trình quét tìm thấy xung đột giữa các cặp chưa được giải quyết trong hai cột đầu tiên. Mỗi cột xung đột sẽ bị xóa ngay lập tức, trong khi thuật toán tiếp tục kiểm tra các cột sau với các so sánh chưa được giải quyết còn lại. Điều này xác nhận rằng mỗi quyết định xóa chỉ dựa trên thông tin mà các cột trong tương lai không thể sửa chữa được.
