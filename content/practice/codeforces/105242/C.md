---
title: "CF 105242C - Dây đàn mạnh mẽ"
description: "Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Mỗi trường hợp thử nghiệm mô tả một cây, nghĩa là một biểu đồ tuần hoàn được kết nối. Nhiệm vụ là quyết định xem có tồn tại một bước đi trên cây này truy cập vào mọi nút ít nhất một lần và không bao giờ nhiều hơn hai lần hay không."
date: "2026-06-24T11:10:15+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105242
codeforces_index: "C"
codeforces_contest_name: "The 2024 Damascus University Collegiate Programming Contest (DCPC 2024)"
rating: 0
weight: 105242
solve_time_s: 51
verified: true
draft: false
---

[CF 105242C - Chuỗi mạnh mẽ](https://codeforces.com/problemset/problem/105242/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số trường hợp thử nghiệm độc lập. Mỗi trường hợp thử nghiệm mô tả một cây, nghĩa là một biểu đồ tuần hoàn được kết nối. Nhiệm vụ là quyết định xem có tồn tại một bước đi trên cây này truy cập vào mọi nút ít nhất một lần và không bao giờ nhiều hơn hai lần hay không. Quá trình đi bộ được phép bắt đầu tại bất kỳ nút nào và nút bắt đầu được coi là đã được truy cập tại thời điểm 0. Mỗi bước đi phải di chuyển dọc theo một cạnh. 

Vì vậy, chúng tôi không được yêu cầu xây dựng lối đi mà chỉ để xác định xem liệu lối đi đó có tồn tại hay không. 

Một cách quan trọng để diễn giải lại điều kiện là suy nghĩ về số lần mỗi nút được nhập trong quá trình truyền tải. Vì biểu đồ là một cái cây nên mọi lượt truy cập lặp lại đều xuất phát từ việc quay lui dọc theo các cạnh. Ràng buộc “nhiều nhất hai lần cho mỗi nút” là cực kỳ chặt chẽ: nó cấm truy cập lại một đỉnh sau khi rời khỏi cây con của nó quá nhiều lần, điều này ngay lập tức gợi ý rằng chỉ những hình dạng cây rất hạn chế mới có thể hoạt động. 

Các ràng buộc rất lớn: tổng số nút trên tất cả các trường hợp thử nghiệm lên tới 200.000. Điều này loại trừ bất kỳ giải pháp nào mô phỏng các bước đi, thử tất cả các điểm bắt đầu hoặc thực hiện quay lui theo cấp số nhân. Ngay cả cách tiếp cận bậc hai cho mỗi trường hợp thử nghiệm cũng sẽ quá chậm. Chúng tôi cần một cái gì đó tuyến tính cho mỗi trường hợp thử nghiệm hoặc tệ nhất là tuyến tính trên tất cả các thử nghiệm. 

Một sai lầm ngây thơ là cho rằng bất kỳ cây nào cũng hoạt động vì chúng ta luôn có thể thực hiện chuyến tham quan DFS Euler. Điều đó là sai vì trong quá trình truyền tải DFS tiêu chuẩn, một số nút có thể được truy cập nhiều hơn hai lần. Ví dụ, trong một cây hình ngôi sao, tâm sẽ được viếng thăm nhiều lần trong một lần duyệt toàn bộ. 

Một hiểu lầm phổ biến khác là cho rằng chúng ta luôn có thể chọn đường đi Hamilton và mở rộng nó ra một chút. Nhưng cây thường có sự phân nhánh buộc phải quay trở lại nhiều lần thông qua nút trung tâm. 

Một ví dụ nhỏ phá vỡ trực giác ngây thơ là một ngôi sao: 

đầu vào: 

n = 4 

1 nối với 2, 3, 4 

Một bước đi DFS ngây thơ bắt đầu từ một chiếc lá sẽ là: 

2 → 1 → 3 → 1 → 4 

Nút 1 được truy cập 3 lần, vi phạm điều kiện. Câu trả lời đúng ở đây là KHÔNG. 

Vì vậy, vấn đề là về hạn chế về cấu trúc của cây cho phép di chuyển với số lần xem lại rất hạn chế. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ sẽ là mô phỏng tất cả các bước đi có thể bắt đầu từ mọi nút, khám phá tất cả các đường dẫn và theo dõi số lượt truy cập. Vì mỗi bước có thể truy cập lại các nút và chúng tôi không bị ràng buộc về độ dài bước đi bên cạnh các ràng buộc ngầm, điều này trở thành một vấn đề không gian trạng thái rất lớn. Ngay cả khi chúng tôi giới hạn độ dài bước đi ở khoảng 2n, các lựa chọn phân nhánh sẽ bùng nổ theo cấp số nhân vì tại mỗi nút, chúng tôi có thể đi đến bất kỳ nút lân cận nào ngoại trừ nút trước đó. 

Điều này không thành công vì trạng thái phải bao gồm cả nút hiện tại và vectơ số lượt truy cập trên tất cả các nút, quá lớn. 

Cái nhìn sâu sắc quan trọng là chuyển quan điểm từ “xây dựng một cuộc đi bộ” sang “cây áp đặt ràng buộc cấu trúc nào nếu mỗi nút được truy cập nhiều nhất hai lần”. 

Bước đi trên cây về cơ bản là một trình tự đi qua các cạnh. Mỗi lần chúng ta vào một nút, chúng ta sẽ tiếp tục đi sâu hơn hoặc cuối cùng quay trở lại cùng một cạnh. Cách duy nhất để giữ số lượt truy cập ở mức thấp là đảm bảo rằng cây không tạo ra các “điểm giao cắt trung tâm” lặp đi lặp lại. 

Quan sát quan trọng là nếu một nút có cấp độ 3 trở lên thì bất kỳ bước đi nào bao phủ tất cả các nhánh đều phải đi qua nút đó nhiều lần theo cách vượt quá giới hạn cho phép. Chính xác hơn, một nút có bậc ≥ 3 tạo ra ít nhất ba cây con rời rạc và bất kỳ bước đi nào bao gồm tất cả chúng đều phải quay lại nút đó nhiều lần. Người ta có thể chính thức hóa rằng một nút như vậy buộc phải có ít nhất ba lượt truy cập trong bất kỳ bước đi bao phủ nào.

Điều này ngay lập tức gợi ý rằng cái cây phải cực kỳ giống con đường. Trên thực tế, những cây duy nhất hoạt động là những cây có đường dẫn đơn giản hoặc “đường dẫn có thêm một điểm nhánh được kiểm soát để số lần truy cập lại không vượt quá hai”. Suy luận cẩn thận cho thấy điều kiện đơn giản hóa việc kiểm tra xem cây là một đường dẫn hay cấu trúc gần đường dẫn trong đó tối đa hai nút có bậc lớn hơn 2 và các nút đó tạo thành một sắp xếp chuỗi đơn. Tuy nhiên, một dẫn xuất tiêu chuẩn và mạnh mẽ hơn cho thấy đặc tính rõ ràng hơn: câu trả lời là CÓ khi và chỉ nếu cây là một đường đi đơn giản. 

Tại sao? Bởi vì bất kỳ điểm phân nhánh nào đều giới thiệu ít nhất ba hướng và để bao phủ tất cả các lá, bước đi phải truy cập lại nút phân nhánh ít nhất nhiều lần bằng số lượng nhánh trừ đi một, ngay lập tức vượt quá giới hạn cho phép. 

Vì vậy, chúng ta chỉ cần kiểm tra xem mọi nút có bậc tối đa là 2 hay không và cây có được kết nối hay không, điều này luôn luôn như vậy. Điều đó có nghĩa là cây phải có nhiều nhất hai nút cấp 1 và tất cả các nút khác cấp 2, chính xác là một đường dẫn. 

Do đó, giải pháp giảm xuống việc kiểm tra xem cây có phải là đồ thị đường dẫn hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng bước đi Brute Force | Hàm mũ | O(n) | Quá chậm | 
| Kiểm tra lộ trình dựa trên bằng cấp | O(n) mỗi lần kiểm tra | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc cây cho từng trường hợp kiểm thử và tính toán cấp độ của mỗi nút. Điều này nắm bắt cấu trúc phân nhánh cục bộ, xác định liệu các lần truy cập bắt buộc có thể xảy ra hay không. 
2. Đếm xem có bao nhiêu nút có bậc chính xác là 1. Đây là những chiếc lá của cây. Trên một con đường phải có đúng hai lá. 
3. Kiểm tra tất cả các nút để đảm bảo không có nút nào có độ lớn hơn 2. Bất kỳ nút nào như vậy đều đưa ra sự phân nhánh buộc phải quay lại nhiều lần trong bất kỳ bước đi bao phủ nào. 
4. Nếu số lá chính xác là 2 và không có nút nào có bậc lớn hơn 2, thì xuất CÓ, nếu không thì xuất NO. 

Lý do điều này có hiệu quả là vì một cây có tối đa 2 bậc nhất thiết phải là một đường dẫn. Điểm cuối của đường dẫn chính xác là các nút có độ 1. Bất kỳ sai lệch nào so với cấu trúc này sẽ tạo ra một điểm phân nhánh buộc phải truy cập lại vượt quá giới hạn cho phép. 

### Tại sao nó hoạt động 

Trong một cây mà mọi nút có nhiều nhất là 2, có chính xác một chuỗi đơn giản kết nối tất cả các nút. Bất kỳ bước đi nào bắt đầu từ một điểm cuối đều có thể đi qua chuỗi đến điểm cuối khác, truy cập từng nút chính xác một lần. Điều này thỏa mãn ràng buộc ngay lập tức. 

Nếu bất kỳ nút nào có bậc ít nhất là 3 thì nút đó sẽ chia cây thành nhiều nhánh. Để bao phủ tất cả các nhánh trong một bước đi được kết nối, quá trình truyền tải phải liên tục quay trở lại nút đó, tăng số lượt truy cập của nó lên hơn hai. Vì vậy, cấu hình như vậy là không thể dưới sự ràng buộc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        deg = [0] * (n + 1)
        
        for _ in range(n - 1):
            u, v = map(int, input().split())
            deg[u] += 1
            deg[v] += 1
        
        leaves = 0
        ok = True
        
        for i in range(1, n + 1):
            if deg[i] > 2:
                ok = False
            if deg[i] == 1:
                leaves += 1
        
        if n == 2:
            print("YES")
        elif ok and leaves == 2:
            print("YES")
        else:
            print("NO")

if __name__ == "__main__":
    solve()
```Việc thực hiện là một bản dịch trực tiếp của điều kiện kết cấu. Mảng độ nắm bắt thông tin kề cận. Vòng lặp trên các cạnh sẽ xây dựng cây. Bước xác thực sẽ kiểm tra cả hai ràng buộc: không phân nhánh vượt quá cấp 2 và chính xác hai điểm cuối. 

Trường hợp đặc biệt n = 2 hoàn toàn là một đường dẫn và logic đã xử lý nó, nhưng nó được giữ rõ ràng để rõ ràng vì cả hai nút đều có bậc 1 và thỏa mãn điều kiện. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào: 

n = 3, các cạnh: 1-2, 2-3 

Bằng cấp: 

1:1, 2:2, 3:1 

| Bước | kiểm tra độ | đếm lá | quyết định | 
| --- | --- | --- | --- | 
| sau khi xây dựng | tất cả 2 | 2 | CÓ | 

Điều này thể hiện một con đường đơn giản. Đi bộ 1 → 2 → 3 ghé thăm mỗi nút đúng một lần, do đó các ràng buộc được thỏa mãn. 

### Ví dụ 2 

đầu vào: 

n = 4, các cạnh: 1-2, 1-3, 1-4 

Bằng cấp: 

1:3, 2:1, 3:1, 4:1 

| Bước | kiểm tra độ | đếm lá | quyết định | 
| --- | --- | --- | --- | 
| sau khi xây dựng | nút 1 có bậc 3 | 3 | KHÔNG | 

Điều này cho thấy một ngôi sao. Bất kỳ bước đi nào bao phủ tất cả các lá đều phải quay lại nút 1 nhiều lần, buộc nó phải được truy cập nhiều hơn hai lần. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi cạnh được xử lý một lần và mỗi nút được kiểm tra một lần | 
| Không gian | O(n) | Mảng độ lưu trữ một số nguyên trên mỗi nút | 

Vì tổng n trên tất cả các trường hợp thử nghiệm nhiều nhất là 200.000, giải pháp tuyến tính này phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    output = []

    input = sys.stdin.readline

    t = int(input())
    for _ in range(t):
        n = int(input())
        deg = [0] * (n + 1)
        for _ in range(n - 1):
            u, v = map(int, input().split())
            deg[u] += 1
            deg[v] += 1

        leaves = 0
        ok = True
        for i in range(1, n + 1):
            if deg[i] > 2:
                ok = False
            if deg[i] == 1:
                leaves += 1

        if n == 2:
            output.append("YES")
        elif ok and leaves == 2:
            output.append("YES")
        else:
            output.append("NO")

    return "\n".join(output)

# provided sample-like tests
assert run("1\n2\n1 2\n") == "YES"
assert run("1\n4\n1 2\n1 3\n1 4\n") == "NO"

# custom cases
assert run("1\n3\n1 2\n2 3\n") == "YES", "simple path"
assert run("1\n5\n1 2\n2 3\n3 4\n4 5\n") == "YES", "long path"
assert run("1\n5\n1 2\n1 3\n1 4\n1 5\n") == "NO", "star"
assert run("1\n4\n1 2\n2 3\n2 4\n") == "NO", "branching center"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đường dẫn cỡ 3 | CÓ | tính đúng đắn cơ bản | 
| chuỗi dài | CÓ | khả năng mở rộng trên đường dẫn | 
| đồ thị sao | KHÔNG | từ chối cấp độ cao | 
| một nút phân nhánh | KHÔNG | cây phi đạo vi tế | 

## Vỏ cạnh 

Trường hợp cạnh khóa là cây hợp lệ nhỏ nhất, n = 2. Thuật toán tính cả hai nút là lá và không thấy bậc nào lớn hơn 2, do đó, nó xuất ra CÓ, khớp với thực tế là một cạnh có thể duyệt qua được một cách bình thường trong các ràng buộc. 

Một trường hợp quan trọng khác là một cái cây gần như là một đường đi nhưng có thêm một nhánh duy nhất, chẳng hạn như 1-2-3-4 có thêm cạnh 2-5. Ở đây nút 2 có cấp độ 3. Thuật toán ngay lập tức loại bỏ nó ở giai đoạn kiểm tra cấp độ. Mọi bước đi hợp lệ sẽ cần phải đi theo 1-2-3-2-4-2-5 hoặc tương tự, buộc nút 2 phải được truy cập ít nhất ba lần. 

Trường hợp tinh tế cuối cùng là một chuỗi dài có thêm một chiếc lá gắn gần điểm cuối. Mặc dù nhìn bề ngoài nó giống một đường dẫn, nhưng chiếc lá thừa đó tạo ra nút cấp 3 và lý do tương tự cũng được áp dụng. Thuật toán phát hiện nó cục bộ mà không cần mô phỏng bất kỳ cấu trúc bước đi nào.
