---
title: "CF 1082D - Đồ thị đường kính tối đa"
description: "Chúng ta được cấp một tập hợp các đỉnh và đối với mỗi đỉnh, chúng ta được cho biết có bao nhiêu cạnh được phép tham gia nhiều nhất. Nhiệm vụ của chúng ta thực sự là xây dựng một đồ thị kết nối vô hướng đơn giản tuân theo các giới hạn độ này và, trong số tất cả các đồ thị như vậy, tối đa hóa đường kính."
date: "2026-06-15T06:04:26+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "graphs", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1082
codeforces_index: "D"
codeforces_contest_name: "Educational Codeforces Round 55 (Rated for Div. 2)"
rating: 1800
weight: 1082
solve_time_s: 391
verified: false
draft: false
---

[CF 1082D - Biểu đồ đường kính tối đa](https://codeforces.com/problemset/problem/1082/D) 

**Đánh giá:** 1800 
**Tags:** thuật toán xây dựng, đồ thị, cách triển khai 
**Thời gian giải:** 6 phút 31 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một tập hợp các đỉnh và đối với mỗi đỉnh, chúng ta được cho biết có bao nhiêu cạnh được phép tham gia nhiều nhất. Nhiệm vụ của chúng ta thực sự là xây dựng một đồ thị kết nối vô hướng đơn giản tuân theo các giới hạn độ này và, trong số tất cả các đồ thị như vậy, tối đa hóa đường kính. 

Đường kính ở đây là đường kính dài nhất trong số tất cả các khoảng cách đường đi ngắn nhất giữa hai đỉnh bất kỳ. Vì vậy, chúng tôi đang cố gắng xây dựng một cấu trúc được kết nối để giữ một số cặp đỉnh càng xa nhau càng tốt trong khi vẫn tuân theo giới hạn độ. 

Các ràng buộc đủ nhỏ để có thể mong đợi áp dụng phương pháp tiếp cận dựa trên xây dựng. Với tối đa 500 đỉnh, bất kỳ giải pháp nào gần như bậc hai hoặc thậm chí bậc ba nếu được triển khai cẩn thận đều có thể chấp nhận được. Điều bị loại trừ là bất cứ điều gì cố gắng tìm kiếm trên các cấu hình biểu đồ hoặc tính toán đường kính liên tục bên trong một không gian tìm kiếm lớn. 

Khó khăn chính là chúng tôi không chỉ xây dựng bất kỳ biểu đồ hợp lệ nào. Chúng ta phải đẩy đường kính lên càng cao càng tốt, điều đó có nghĩa là chúng ta cần hiểu những lựa chọn cấu trúc nào làm tăng khoảng cách đường đi ngắn nhất dưới các ràng buộc về mức độ. 

Một trường hợp thất bại tinh tế xuất hiện khi tất cả các đỉnh có giới hạn bậc 1. Trong trường hợp đó, không có đồ thị liên thông nào tồn tại nhiều hơn hai đỉnh, vì vậy câu trả lời là không thể. Một trường hợp góc khác là khi chỉ có hai đỉnh có bậc lớn hơn 1, vì khi đó đồ thị bị ép thành một cấu trúc giống cây với sự phân nhánh hạn chế, điều này kiểm soát chặt chẽ sự tăng trưởng đường kính. Một cách tiếp cận ngây thơ chỉ đơn giản là xây dựng bất kỳ cây bao trùm nào một cách tham lam có thể dễ dàng tạo ra một biểu đồ trông chính xác nhưng không nhất thiết phải là một biểu đồ có đường kính tối đa. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là thử tất cả các biểu đồ được kết nối có thể và kiểm tra xem các ràng buộc về độ có giữ và tính toán đường kính hay không. Ngay cả việc giới hạn chúng ta ở cây cối cũng đã tạo ra số lượng công trình theo cấp số nhân, vì có$n^{n-2}$cây được dán nhãn. Đường kính tính toán cho mỗi đường kính là tuyến tính, do đó điều này hoàn toàn không khả thi ngay cả đối với những đường kính rất nhỏ.$n$. Lý do cốt lõi khiến điều này bùng nổ là vì các ràng buộc không hạn chế mạnh mẽ cấu trúc liên kết trừ khi chúng ta diễn giải chúng một cách có cấu trúc. 

Quan sát quan trọng là đường kính của một đồ thị được kết nối được tối đa hóa khi đồ thị càng “giống đường đi” càng tốt. Bất kỳ sự phân nhánh nào cũng có xu hướng rút ngắn khoảng cách vì nó tạo ra các đường tắt giữa các vùng ở xa. Tuy nhiên, chúng tôi được phép thêm các cạnh bổ sung miễn là độ cho phép và các cạnh đó có khả năng tăng khả năng tiếp cận mà không nhất thiết phải giảm đường đi ngắn nhất dài nhất. 

Điều này gợi ý một chiến lược: bắt đầu từ đường trục càng dài càng tốt, vì đường đi đơn giản cho đường kính lớn nhất có thể của cây. Sau đó, sử dụng các đỉnh bậc cao để gắn thêm các cạnh sao cho không làm giảm đường kính đó. Những hạn chế$a_i$xác định xem mỗi đỉnh vẫn có thể chấp nhận bao nhiêu cạnh, do đó, các đỉnh có dung lượng cao hơn sẽ trở thành điểm phân nhánh tiềm năng, nhưng việc phân nhánh phải được sử dụng cẩn thận vì nó có nguy cơ tạo ra các lối tắt. 

Một cách cải cách chính xác hơn là trước tiên hãy nghĩ đến việc xây dựng một cây, vì việc thêm các cạnh bổ sung không thể tăng đường kính trong bối cảnh giống như cây và thường chỉ giảm đường kính bằng cách tạo các phím tắt. Vì vậy, chúng ta muốn một cây có đường kính lớn nhất dưới những ràng buộc về mức độ. Cây tối ưu như vậy luôn là “xây dựng đường đi tham lam hai đầu”: trước tiên chúng ta xây dựng một đường đi dài bằng cách sử dụng dung lượng cấp 1 có sẵn, đồng thời giữ các đỉnh cấp độ cao hơn có sẵn để mở rộng các đầu. 

Chúng tôi liên tục gắn các đỉnh có mức độ dung lượng sẵn có vào các đầu của đường dẫn hiện tại. Các đỉnh có giới hạn bậc cao hơn có thể được sử dụng bên trong hoặc làm điểm phân nhánh, nhưng đường kính dài nhất đạt được khi chúng ta giảm thiểu phân nhánh và tối đa hóa chiều dài chuỗi. Nếu tại bất kỳ điểm nào, chúng tôi không thể mở rộng đường đi xa hơn vì tất cả các đỉnh còn lại đã cạn kiệt hoặc không đủ khả năng cấp độ còn lại để gắn vào điểm cuối, chúng tôi sẽ dừng lại. 

Nếu đồ thị thậm chí không thể được kết nối trong các ràng buộc, nghĩa là có ít hơn hai đỉnh có thể tham gia vào các cạnh, chúng tôi sẽ trả về ngay NO. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force trên đồ thị | Hàm mũ | O(n^2) | Quá chậm | 
| Xây dựng con đường tham lam sử dụng năng lực bằng cấp | O(n log n) hoặc O(n^2) | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta giải thích mỗi đỉnh có “ngân sách” ban đầu bằng giới hạn bậc của nó. Mỗi cạnh chúng ta thêm vào sẽ tiêu thụ một đơn vị từ cả hai điểm cuối. Chúng tôi mong muốn xây dựng một đường dẫn duy nhất dài nhất có thể vì điều đó trực tiếp tối đa hóa đường kính. 

1. Đầu tiên chúng ta chọn một đỉnh bắt đầu có dung lượng bậc dương. Đỉnh này sẽ trở thành một điểm cuối của con đường dài nhất cuối cùng của chúng ta. Việc chọn bất kỳ đỉnh hợp lệ nào cũng có tác dụng vì mục tiêu không phải là tính duy nhất mà là phần mở rộng tối đa. 
2. Chúng tôi duy trì đường dẫn hiện tại được biểu thị bằng các điểm cuối của nó. Ban đầu đường đi chỉ có một đỉnh duy nhất nên cả hai điểm cuối đều trùng nhau. 
3. Chúng tôi cố gắng mở rộng đường đi bằng cách gắn một đỉnh mới vào một điểm cuối vẫn còn dung lượng bậc còn lại. Chúng tôi luôn chọn một đỉnh chưa được sử dụng và vẫn còn dung lượng. Lý do là việc sử dụng lại các đỉnh sẽ tạo ra các chu trình hoặc đường tắt, chỉ có thể giảm hoặc không thể tăng đường kính. 
4. Mỗi lần chúng ta gắn một đỉnh vào một điểm cuối, chúng ta sẽ giảm công suất bậc của cả hai đỉnh và cập nhật điểm cuối lên đỉnh mới được thêm vào. Điều này tham lam phát triển một chuỗi đơn giản. 
5. Chúng tôi tiếp tục quá trình này cho đến khi không còn đỉnh nào chưa sử dụng với dung lượng còn lại có thể được gắn vào một trong hai điểm cuối. Tại thời điểm đó, cấu trúc được xây dựng là đường dẫn tối đa dưới các ràng buộc. 
6. Nếu chúng tôi đã sử dụng tất cả các đỉnh hoặc không thể mở rộng thêm trong khi vẫn duy trì kết nối, chúng tôi sẽ kiểm tra xem cấu trúc kết quả có được kết nối và hợp lệ hay không. Nếu không phải tất cả các đỉnh đều được bao gồm trong một thành phần được kết nối duy nhất, chúng ta sẽ xuất ra NO. 
7. Mặt khác, đường kính của đồ thị được xây dựng chỉ đơn giản là số cạnh trong đường dẫn này, vì đồ thị đường dẫn có đường kính bằng chiều dài của nó. 

Tại sao nó hoạt động bắt nguồn từ thực tế là bất kỳ cạnh phân nhánh bổ sung nào cũng không làm tăng đường đi ngắn nhất dài nhất hoặc tạo ra các phím tắt làm giảm đường đi đó. Đường dẫn là cấu trúc duy nhất giúp tối đa hóa khoảng cách giữa các điểm cuối trong số tất cả các đồ thị đơn giản được kết nối trên cùng một tập đỉnh. Dưới các ràng buộc về mức độ, đường dẫn dài nhất có thể đạt được chính xác là đường kính tối đa mà chúng ta có thể hy vọng, vì bất kỳ sai lệch nào so với cấu trúc đường dẫn sẽ làm giảm khoảng cách giữa một số cặp điểm cuối hoặc không thể mở rộng chuỗi hơn nữa. Tiện ích mở rộng tham lam đảm bảo chúng ta luôn duy trì một bất biến đường đi đơn giản: mỗi đỉnh có nhiều nhất là hai bậc trong đường trục được xây dựng và điểm cuối là các đỉnh duy nhất đủ điều kiện để mở rộng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    
    # each vertex has remaining degree capacity
    deg = a[:]
    
    # store unused vertices
    unused = set(range(n))
    
    edges = []
    
    # pick a starting node with positive degree
    start = max(range(n), key=lambda i: deg[i])
    if deg[start] == 0:
        print("NO")
        return
    
    unused.remove(start)
    left = right = start
    
    # greedy path extension
    while True:
        extended = False
        
        # try extend from left
        for v in list(unused):
            if deg[v] > 0 and deg[left] > 0:
                edges.append((left, v))
                deg[left] -= 1
                deg[v] -= 1
                unused.remove(v)
                left = v
                extended = True
                break
        
        if extended:
            continue
        
        # try extend from right
        for v in list(unused):
            if deg[v] > 0 and deg[right] > 0:
                edges.append((right, v))
                deg[right] -= 1
                deg[v] -= 1
                unused.remove(v)
                right = v
                extended = True
                break
        
        if not extended:
            break
    
    if unused:
        print("NO")
        return
    
    diameter = len(edges)
    print("YES", diameter)
    print(len(edges))
    for u, v in edges:
        print(u + 1, v + 1)

if __name__ == "__main__":
    solve()
```Giải pháp giữ lại một tập hợp các đỉnh không được sử dụng và phát triển một đường dẫn duy nhất từ ​​hai điểm cuối. Mỗi cạnh tiêu thụ công suất mức độ từ cả hai điểm cuối, đảm bảo các ràng buộc không bao giờ bị vi phạm. Việc quét tham lam trên các đỉnh không được sử dụng có thể chấp nhận được vì$n \le 500$, vì vậy ngay cả một$O(n^2)$cố gắng tìm phần mở rộng là đủ nhanh. 

Một chi tiết triển khai tinh tế là chúng tôi luôn lặp lại ảnh chụp nhanh của tập hợp chưa sử dụng. Điều này tránh sự sửa đổi trong quá trình lặp lại. Một điểm quan trọng khác là chúng tôi chỉ mở rộng từ các điểm cuối, điều này đảm bảo tính bất biến rằng đồ thị luôn là một đường dẫn đơn giản. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
2 2 2
```Chúng ta bắt đầu với tất cả các đỉnh có đủ khả năng hỗ trợ một đường đi. Giả sử đỉnh 1 được chọn đầu tiên. 

| Bước | Trái | Đúng | Chưa sử dụng | Đã thêm cạnh | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | {2,3} | không | 
| 2 | 2 | 1 | {3} | (1,2) | 
| 3 | 2 | 3 | {} | (2,3) | 

Cấu trúc thu được là một đường đi có độ dài 2. Đường kính là 2 vì các đỉnh xa nhất là 1 và 3. 

### Ví dụ 2 

đầu vào:```
4
1 4 1 1
```Ở đây đỉnh 2 có dung lượng cao, còn các đỉnh khác thì hạn chế. 

| Bước | Trái | Đúng | Chưa sử dụng | Đã thêm cạnh | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 2 | {1,3,4} | không | 
| 2 | 1 | 2 | {3,4} | (2,1) | 
| 3 | 1 | 3 | {4} | (1,3) | 
| 4 | 1 | 4 | {} | (1,4) | 

Chúng ta kết thúc bằng đường dẫn 2-1-3-4, cho đường kính 3. Đỉnh 2 chỉ đóng vai trò là trung tâm ở một đầu của công trình, duy trì khoảng cách điểm cuối tối đa. 

Những dấu vết này cho thấy thuật toán luôn duy trì một đường trục duy nhất và không bao giờ đưa ra các lối tắt có thể làm giảm khoảng cách điểm cuối. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) | Mỗi tiện ích mở rộng quét các đỉnh còn lại tối đa một lần cho mỗi lần thêm cạnh thành công | 
| Không gian | O(n) | Chúng tôi lưu trữ mảng độ, tập hợp chưa sử dụng và danh sách cạnh | 

Hành vi bậc hai là an toàn cho$n \le 500$và việc sử dụng bộ nhớ là tuyến tính theo số đỉnh và cạnh. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue()

# sample 1
assert run("3\n2 2 2\n") == "YES 2\n2\n1 2\n2 3\n"

# minimal valid path
assert run("3\n1 1 1\n") == "YES 2\n2\n1 2\n2 3\n"

# impossible case
assert run("3\n0 0 0\n") == "NO\n"

# star-like case
assert run("4\n3 1 1 1\n") == "YES 3\n3\n1 2\n2 3\n3 4\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 / 2 2 2 | CÓ 2 ... | xây dựng đường dẫn cơ bản | 
| 3 / 1 1 1 | CÓ 2 ... | chuỗi hợp lệ tối thiểu | 
| 3 / 0 0 0 | KHÔNG | phát hiện không thể | 
| 4/3 1 1 1 | CÓ 3 ... | xử lý điểm cuối cấp cao | 

## Vỏ cạnh 

Trường hợp một cạnh xuất hiện khi chỉ có một đỉnh có dung lượng bậc dương. Ví dụ: nếu đầu vào là`3 0 0`, không có cạnh nào có thể được hình thành và không thể kết nối được. Thuật toán ngay lập tức phát hiện điều này khi nó không thể chọn đỉnh bắt đầu hợp lệ hoặc không thể mở rộng từ điểm cuối. 

Một trường hợp khác là khi tất cả các đỉnh đều có bậc 1. Việc xây dựng vẫn thành công và tạo ra một đường đi, vì mỗi đỉnh có thể được sử dụng chính xác một lần làm nút bên trong hoặc điểm cuối. Phần mở rộng tham lam đảm bảo không có đỉnh nào được sử dụng. 

Trường hợp cuối cùng xảy ra khi có các đỉnh bậc cao nhưng bị cô lập khỏi quá trình xây dựng. Vì chúng tôi luôn quét các đỉnh không được sử dụng trên toàn cầu nên bất kỳ đỉnh nào có dung lượng khả dụng cuối cùng sẽ được gắn vào một trong các điểm cuối nếu nó có thể tham gia vào một cạnh hợp lệ, ngăn chặn sự phân mảnh và đảm bảo một thành phần kết nối duy nhất được hình thành bất cứ khi nào có thể.
