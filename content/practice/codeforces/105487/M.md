---
title: "CF 105487M - Che cây"
description: "Một cây được đưa ra dưới dạng biểu diễn gốc, vì vậy mọi nút ngoại trừ nút gốc đều có một nút cha duy nhất và các cạnh được hướng ngầm hướng lên trên về phía nút gốc đó. Chúng ta được yêu cầu bao phủ từng cạnh của cây chính xác một lần bằng cách sử dụng một số đoạn được định hướng."
date: "2026-06-23T19:08:28+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105487
codeforces_index: "M"
codeforces_contest_name: "2024 China Collegiate Programming Contest (CCPC) Female Onsite (2024\u5e74\u4e2d\u56fd\u5927\u5b66\u751f\u7a0b\u5e8f\u8bbe\u8ba1\u7ade\u8d5b\u5973\u751f\u4e13\u573a)"
rating: 0
weight: 105487
solve_time_s: 67
verified: true
draft: false
---

[CF 105487M - Che cây](https://codeforces.com/problemset/problem/105487/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 7s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Một cây được đưa ra dưới dạng biểu diễn gốc, vì vậy mọi nút ngoại trừ nút gốc đều có một nút cha duy nhất và các cạnh được hướng ngầm hướng lên trên về phía nút gốc đó. Chúng ta được yêu cầu bao phủ từng cạnh của cây chính xác một lần bằng cách sử dụng một số đoạn được định hướng. Mỗi đoạn phải bắt đầu ở một lá và sau đó di chuyển lên trên dọc theo các liên kết cha mẹ cho đến khi nó dừng lại ở một tổ tiên nào đó. Các phân đoạn khác nhau không thể chồng lên nhau trên các cạnh và mỗi cạnh phải thuộc chính xác một phân đoạn. 

Một cách khác để xem điều này là chúng ta phân tách tất cả các đường dẫn gốc thành một tập hợp các chuỗi từ lá tới tổ tiên, trong đó mỗi chuỗi được phép dừng sớm ở bất kỳ tổ tiên nào, nhưng sự kết hợp của tất cả các chuỗi phải phân chia tập cạnh của cây. Trong số tất cả các phân tách như vậy, chúng ta quan tâm đến độ dài tối đa của bất kỳ chuỗi nào, được đo bằng các cạnh, và chúng ta muốn mức tối đa này càng nhỏ càng tốt. 

Các ràng buộc thúc đẩy mạnh mẽ tới một giải pháp gần tuyến tính. Tổng số nút trên tất cả các trường hợp thử nghiệm tối đa là 2 × 10^5, do đó, bất kỳ giải pháp nào thậm chí hơi siêu tuyến tính cho mỗi trường hợp thử nghiệm sẽ chỉ vượt qua nếu nó được khấu hao tuyến tính. Một cách tiếp cận ngây thơ cố gắng liệt kê hoặc mô phỏng các lựa chọn phân đoạn một cách rõ ràng trên mỗi cạnh hoặc trên mỗi điểm cuối có thể sẽ ngay lập tức thất bại, vì ngay cả O(n log n) cho mỗi trường hợp thử nghiệm cũng có thể trở nên chặt chẽ nếu được thực hiện bất cẩn nhưng vẫn được chấp nhận trên toàn cầu. 

Một vấn đề nhỏ xuất hiện trong cách các phân đoạn tương tác tại các nút nội bộ. Hãy xem xét một nút có nhiều cây con. Nhiều phân đoạn đến từ các nút con khác nhau có thể đến nút này, nhưng chỉ một phân đoạn có thể tiếp tục đi lên qua cạnh tới nút cha của nó, vì cạnh đó phải được sử dụng đúng một lần. Tất cả các phân đoạn khác đến nút này phải kết thúc ở đây. Một ý tưởng tham lam ngây thơ như “luôn mở rộng mọi thứ lên trên” không thành công vì nó sẽ vi phạm tính duy nhất của việc sử dụng cạnh phía trên nút. Một ý tưởng sai lầm khác là tự ý chấm dứt các phân đoạn ngay khi chúng gặp nhau, điều này có thể làm tăng độ dài phân đoạn tối đa trong các cây con trong đó việc trì hoãn việc kết thúc sẽ tốt hơn. 

Một trường hợp thất bại minh họa nhỏ cho trực giác ngây thơ là một cây hình ngôi sao có tâm 1 và các lá 2, 3, 4. Nếu chúng ta luôn mở rộng tất cả các đường dẫn lá lên trên, thì tất cả các đoạn đi 2→1, 3→1, 4→1, hợp lệ và tối ưu với độ dài tối đa 1. Nhưng trong cấu trúc giống chuỗi sâu hơn, việc kết thúc sớm một đường dẫn dài ở mức thấp trong cây có thể buộc một đường dẫn khác kế thừa độ sâu lớn còn lại sau đó, làm tăng độ dài đoạn tối đa một cách bất ngờ. Quyết định phải được phối hợp tại mỗi nút chứ không phải cục bộ trên mỗi cạnh. 

## Phương pháp tiếp cận 

Chiến lược bạo lực sẽ cố gắng xây dựng phân đoạn một cách rõ ràng. Tại mỗi nút, chúng ta có thể thử tất cả các cách có thể để quyết định đường dẫn con nào tiếp tục đi lên và đường dẫn nào kết thúc, khám phá các kết hợp đệ quy. Mỗi đứa trẻ đóng góp một lựa chọn, do đó, nút cấp d tạo ra các khả năng 2^d cục bộ và các quyết định này tương tác trên toàn bộ cây. Ngay cả khi chúng ta cắt bớt theo tính khả thi, không gian tìm kiếm toàn cục sẽ trở thành hàm mũ theo số lượng nút, vì mỗi nút đưa ra các quyết định phân nhánh lan truyền lên trên qua các cạnh được chia sẻ. Điều này nhanh chóng trở nên không khả thi ngay cả với n khoảng 30, chứ đừng nói đến 2×10^5. 

Quan sát cấu trúc quan trọng là cây thực thi một ràng buộc rất cứng nhắc: đối với mọi nút ngoại trừ nút gốc, chính xác một đường dẫn từ cây con của nó được phép đi qua nút đó tới nút cha. Tất cả các đường dẫn khác phải kết thúc ở hoặc bên dưới nó. Điều này chuyển đổi vấn đề từ tổ hợp toàn cục thành vấn đề lựa chọn cục bộ tại mỗi nút: trong số tất cả các đường đi lên ứng cử viên, chúng ta phải quyết định đường đi đơn nào được “thúc đẩy” đi lên.

Một khi chúng ta nhìn vấn đề theo cách này, mục tiêu sẽ trở nên rõ ràng. Mỗi đường dẫn ứng cử viên có độ dài hiện tại từ nút lá của nó đến nút hiện tại. Nếu chúng ta chọn một con đường để tiếp tục, độ dài của nó sẽ tăng thêm một khi truyền tới cha mẹ, do đó, việc tiếp tục một con đường dài là rất rủi ro vì sau này nó có thể trở thành mức tối đa. Do đó, tại mỗi nút, chúng tôi muốn chuyển lên trên con đường ngắn nhất có sẵn và buộc những con đường dài hơn phải kết thúc ở đây, vì việc kết thúc sẽ đóng băng độ dài của chúng ở giá trị nhỏ hơn. 

Sự lựa chọn tham lam này hóa ra là đủ và nó cho phép chúng ta xử lý cây từ dưới lên bằng cách sử dụng DFS, duy trì tại mỗi nút danh sách độ dài đường dẫn đến từ các nút con. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force về các quyết định phân khúc | Hàm mũ | O(n) | Quá chậm | 
| DFS tham lam từ dưới lên | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý cây bằng cách sử dụng phương pháp truyền tải theo chiều sâu, tính toán cho mỗi nút một tập hợp độ dài đường dẫn đi lên bắt nguồn từ cây con của nó. 

1. Thực hiện DFS từ thư mục gốc. Đối với mỗi nút, tính toán đệ quy danh sách độ dài đường dẫn được mỗi nút con trả về, trong đó mỗi độ dài biểu thị một đường dẫn bắt đầu từ một lá trong cây con đó và đến nút hiện tại. 
2. Khi trả về từ nút con, hãy tăng mỗi độ dài được trả về thêm một để tính đến cạnh giữa nút con và nút hiện tại. Điều này thể hiện việc nâng tất cả các đường dẫn lên một cấp. 
3. Thu thập tất cả độ dài đường đi như vậy từ tất cả trẻ em vào một danh sách duy nhất. Mỗi phần tử đại diện cho một phân đoạn ứng viên hiện đang kết thúc tại nút hiện tại. 
4. Nếu nút không phải là nút gốc và có ít nhất một đường dẫn ứng cử viên, hãy chọn độ dài nhỏ nhất trong số chúng và đánh dấu nó là đường sẽ tiếp tục đi lên. Lý do là việc mở rộng đường đi nhỏ nhất sẽ giảm thiểu rủi ro tạo ra một đoạn lớn sau này khi các cạnh khác được thêm vào ở trên. 
5. Tất cả các đường dẫn còn lại tại nút này đều được kết thúc tại đây. Mỗi đường dẫn kết thúc đóng góp độ dài cuối cùng của nó vào câu trả lời chung. 
6. Đường đi nhỏ nhất đã chọn sẽ được trả về phía cha mẹ để nó có thể tiếp tục đi qua các cạnh cao hơn. 
7. Tại gốc rễ, không có con đường nào cần tiếp tục đi lên. Tất cả các đường dẫn đều bị kết thúc và độ dài của chúng được coi là cuối cùng. 

Bất biến trung tâm là mọi nút đều thực thi quy tắc rằng chính xác một đường dẫn từ cây con của nó được phép đi lên qua cạnh cha của nó và đường dẫn đã chọn đó luôn là đường dẫn ngắn nhất có sẵn tại thời điểm đó. Điều này đảm bảo rằng các đường dẫn dài hơn luôn được hoàn thành sớm nhất có thể, ngăn chúng tích lũy thêm độ dài một cách không cần thiết. Bởi vì các quyết định kết thúc là không thể hủy ngang, việc trì hoãn việc kết thúc đối với một đường dẫn dài hơn chỉ có thể làm tăng mức tối đa cuối cùng, trong khi việc trì hoãn đối với đường đi ngắn nhất sẽ giảm thiểu sự tăng trưởng trong trường hợp xấu nhất khi cuối cùng nó được mở rộng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(10**7)

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        parents = [0] + list(map(int, input().split()))
        
        g = [[] for _ in range(n + 1)]
        for i in range(2, n + 1):
            g[parents[i]].append(i)

        ans = 0

        def dfs(u):
            nonlocal ans
            paths = []

            for v in g[u]:
                child_paths = dfs(v)
                for x in child_paths:
                    paths.append(x + 1)

            if not paths:
                return [0]

            if u == 1:
                for x in paths:
                    ans = max(ans, x)
                return []

            paths.sort()
            ans = max(ans, paths[-1])

            return [paths[0]]

        dfs(1)
        print(ans)

if __name__ == "__main__":
    solve()
```DFS trả về một danh sách chứa nhiều nhất một phần tử, thể hiện đường dẫn duy nhất được phép tiếp tục đi lên từ cây con. Các nút lá trả về một đường dẫn có độ dài bằng 0. Khi hợp nhất các phần tử con, mỗi đường đi đều được tăng lên để tính đến việc truyền tải cạnh. 

Tại mỗi nút bên trong, việc sắp xếp được sử dụng để xác định đường dẫn ứng cử viên nhỏ nhất để truyền lên trên. Tất cả các đường dẫn khác ngay lập tức được hoàn thiện bằng cách cập nhật mức tối đa toàn cầu. Gốc chỉ khác ở chỗ nó không thể truyền bất kỳ con đường đi lên nào, vì vậy tất cả các ứng cử viên đều được hoàn thiện ở đó. 

Chi tiết triển khai quan trọng là mỗi nút chỉ duy trì một số lượng nhỏ đường dẫn hoạt động, thường là một đường dẫn cho mỗi cây con sau khi tổng hợp, đảm bảo rằng mặc dù việc sắp xếp được sử dụng cục bộ nhưng độ phức tạp tổng thể vẫn gần tuyến tính. 

## Ví dụ đã hoạt động 

Hãy xem xét một cây nhỏ trong đó nút 1 là gốc và nó có con 2 và 3, còn nút 2 có con 4. 

đầu vào:```
1
4
1 1 2
```Chúng tôi theo dõi lợi nhuận của DFS như sau. 

| Nút | Đường dẫn con đến (sau +1) | Được chọn để tiếp tục | Hoàn thiện tại nút | trả lời | 
| --- | --- | --- | --- | --- | 
| 4 | [0] | trả về [0] | không | 0 | 
| 2 | [1] | [1] tiếp tục | không | 0 | 
| 1 | [2,1] | không | 2 | 2 | 

Tại nút 2, con đường duy nhất được truyền lên trên. Tại gốc 1, cả hai đường dẫn đều được hoàn thiện và dài nhất là 2. 

Điều này cho thấy rằng gốc đóng vai trò là người thu thập cuối cùng của tất cả các phân đoạn còn lại. 

Bây giờ hãy xem xét một cấu trúc phân nhánh:```
1
├──2
│   └──4
└──3
```đầu vào:```
1
4
1 1 2
```Cấu trúc tương tự nhưng tập trung vào phân nhánh ở gốc. 

| Nút | Đường dẫn đến | Tiếp tục | Đã hoàn thiện | trả lời | 
| --- | --- | --- | --- | --- | 
| 2 | [0] | [0] | không | 0 | 
| 3 | [0] | [0] | không | 0 | 
| 1 | [1,1] | không | 1 | 1 | 

Mỗi lá đóng góp một đường dẫn đơn vị và cả hai đều kết thúc ở gốc, giữ độ dài đoạn tối đa ở mức tối thiểu. 

Những dấu vết này nêu bật cách thuật toán chỉ trì hoãn đường dẫn nhỏ nhất có thể và hoàn thành tất cả các đường dẫn khác ngay lập tức. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Mỗi nút hợp nhất các danh sách con và sắp xếp tối đa số lượng đường dẫn đến; trên cây, điều này vẫn gần như tuyến tính do mỗi đường dẫn được xử lý một lần cho mỗi cấp độ | 
| Không gian | O(n) | danh sách kề và ngăn xếp đệ quy cộng với danh sách đường dẫn tạm thời | 

Tổng số nút trên tất cả các trường hợp thử nghiệm được giới hạn bởi 2×10^5, do đó, cách tiếp cận O(n log n) phù hợp thoải mái trong giới hạn ngay cả với nhiều trường hợp thử nghiệm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# single edge
assert run("""1
2
1""") == "1"

# chain
assert run("""1
5
1 2 3 4""") == "4"

# star
assert run("""1
5
1 1 1 1""") == "1"

# balanced tree
assert run("""1
7
1 1 2 2 3 3""") == "2"

# multiple tests
assert run("""2
2
1
3
1 1""") == "1\n1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cạnh đơn | 1 | tính đúng đắn của trường hợp cơ sở | 
| chuỗi | 4 | hành vi độ sâu tối đa | 
| ngôi sao | 1 | chấm dứt sớm tối ưu | 
| cây cân đối | 2 | hợp nhất nhiều cây con | 
| nhiều bài kiểm tra | 1 1 | xử lý các vòng T | 

## Vỏ cạnh 

Chuỗi suy biến kiểm tra xem thuật toán có trì hoãn việc kết thúc không chính xác hay không. Đối với cây 1-2-3-4-5, mỗi nút đi qua chính xác một đường đi lên, do đó không có quyết định sắp xếp nào quan trọng và mỗi cạnh đều tăng độ dài đường đi thêm một. Thuật toán tạo chính xác một đường dẫn cuối cùng có độ dài 4 ở gốc, khớp với mức tối đa dự kiến. 

Một ngôi sao thuần túy kiểm tra xem thuật toán có tránh được sự lan truyền không cần thiết hay không. Tại mỗi lá, một đường dẫn có độ dài bằng 0 được tạo ra và gửi ngay đến gốc. Vì tất cả các đường dẫn đều gặp nhau tại gốc nên chúng đều kết thúc ở đó và giá trị lớn nhất vẫn là 1. Mọi nỗ lực truyền bá nhiều đường đi lên sẽ không cần thiết nhưng vẫn vô hại; thuộc tính quan trọng là không có đường dẫn nào được mở rộng ngoài một cạnh và thuật toán duy trì điều này bằng cách không bao giờ buộc phải tiếp tục ngoại trừ ứng cử viên nhỏ nhất tại mỗi nút.
