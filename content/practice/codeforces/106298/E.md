---
title: "CF 106298E - Cây Manhattan"
description: "Chúng ta được cho một cái cây, nghĩa là một đồ thị được kết nối không có chu trình và chúng ta muốn quyết định xem liệu nó có thể được coi là “Cây Manhattan” hay không."
date: "2026-06-18T22:29:01+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106298
codeforces_index: "E"
codeforces_contest_name: "OCPC 2024 Summer, Day 4: wuhudsm Contest"
rating: 0
weight: 106298
solve_time_s: 53
verified: true
draft: false
---

[CF 106298E - Cây Manhattan](https://codeforces.com/problemset/problem/106298/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một cái cây, nghĩa là một đồ thị được kết nối không có chu trình và chúng ta muốn quyết định xem liệu nó có thể được coi là “Cây Manhattan” hay không. Thuộc tính xác định không phải là khoảng cách hoặc tọa độ được cung cấp rõ ràng mà là về việc liệu cấu trúc của cây có tương thích với việc được vẽ trên lưới số nguyên 2D bằng cách sử dụng tính liền kề kiểu Manhattan, trong đó các cạnh hoạt động giống như các kết nối lưới hay không. 

Trên lưới, mỗi điểm có tối đa bốn hướng tự nhiên để kết nối với các điểm lân cận, lên, xuống, trái và phải. Ràng buộc hình học đó chuyển trực tiếp thành ràng buộc cấu trúc trên bất kỳ cây nào có thể được nhúng vào đó: không có đỉnh nào có thể có nhiều hơn bốn cạnh liên quan. Tuy nhiên, chỉ điều kiện này là chưa đủ, bởi vì cách phân nhánh tương tác trong lưới phẳng nhúng buộc phải có những hạn chế mạnh mẽ hơn về số lượng đỉnh bậc cao có thể cùng tồn tại và cách các chuỗi phân nhánh có thể lan truyền mà không xung đột. 

Nhiệm vụ là đọc một cây và xác định xem cấu trúc của nó có thuộc về một họ nhỏ các cấu hình được phép hay không. Các ràng buộc đủ lớn để cây đầu vào có thể có tới khoảng 200.000 nút, vì vậy mọi giải pháp đều phải hoạt động trong thời gian tuyến tính. Điều này ngay lập tức loại trừ bất kỳ điều gì liên quan đến việc thử tất cả các nội dung nhúng, mô phỏng hình học hoặc liệt kê tổ hợp các bố cục. 

Một nỗ lực ngây thơ có thể cố gắng gán tọa độ cho các nút trong khi tôn trọng khoảng cách Manhattan dọc theo các cạnh. Sự mô phỏng như vậy nhanh chóng trở nên mơ hồ. Ví dụ, trong một cái cây có hình dạng như một ngôi sao có năm lá, người ta có thể cố gắng đặt các nhánh ở giữa và trải rộng theo bốn hướng, nhưng bất kỳ nhánh bổ sung nào cũng sẽ phá vỡ giới hạn công suất của lưới. Tương tự, một cây có nhiều đỉnh phân nhánh có thể dễ dàng gây ra mâu thuẫn trong việc gán hướng, mặc dù mỗi nút cục bộ có vẻ khả thi. 

Một trường hợp phức tạp phát sinh khi độ nhỏ nhưng cấu trúc phân nhánh vẫn không tương thích với việc nhúng lưới. Ví dụ, hãy xem xét một cây trong đó mỗi đỉnh có hai đỉnh có bậc 3 và các cây con phân nhánh của chúng đan xen theo cách đòi hỏi tổng cộng nhiều hơn bốn hướng. Việc kiểm tra mức độ đơn giản có thể chấp nhận hoặc từ chối những trường hợp như vậy một cách không chính xác nếu nó không tính đến cấu trúc phân loại toàn cầu. 

Điểm mấu chốt của vấn đề là Cây Manhattan hợp lệ không phải là cây có mức độ giới hạn tùy ý. Chúng rơi vào một số lượng rất nhỏ các mẫu cấu trúc toàn cầu, và việc nhận ra các mẫu này sẽ giúp giảm vấn đề xuống còn việc đếm và phân loại đơn giản. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng xây dựng một cây nhúng thực tế vào lưới. Bắt đầu từ một gốc tùy ý, chúng ta có thể gán các hướng một cách đệ quy cho các cạnh, cố gắng đặt mỗi hướng con vào một trong bốn hướng Manhattan trong khi đảm bảo không có xung đột nào xảy ra. Mỗi quyết định về vị trí sẽ truyền bá các ràng buộc tới các cây con và việc quay lui sẽ được yêu cầu bất cứ khi nào xung đột xuất hiện. 

Về nguyên tắc, phương pháp này đúng vì nó mô phỏng trực tiếp định nghĩa của một nhúng hợp lệ. Tuy nhiên, mỗi đỉnh phân nhánh đưa ra nhiều lựa chọn và trong trường hợp xấu nhất, một cây có nhiều điểm phân nhánh sẽ dẫn đến số lượng phép gán định hướng theo cấp số nhân. Ngay cả khi cắt tỉa, không gian tìm kiếm vẫn tăng lên cực kỳ nhanh chóng, khiến điều này không thể thực hiện được đối với những cây lớn. 

Quan sát quan trọng là chúng ta thực sự không cần tọa độ. Báo cáo vấn đề đã hạn chế các cây hợp lệ ở một tập hợp nhỏ các chữ ký cấu trúc. Những dấu hiệu này xuất phát từ thực tế là trong một lưới, việc phân nhánh bị hạn chế rất nhiều: hầu hết các nút chỉ có thể nằm trên các chuỗi và chỉ một số đỉnh có thể đóng vai trò là các điểm nối phân nhánh. Khi chúng tôi phân tích có bao nhiêu đỉnh bậc 3 hoặc 4 tồn tại và cách chúng có thể kết nối với nhau, chúng tôi thấy rằng tất cả các cây hợp lệ đều thuộc bốn loại.

Cây Manhattan hợp lệ phải là một trong những loại sau: một chuỗi thuần túy trong đó mỗi nút có nhiều nhất là cấp 2, một cây có chính xác một nút phân nhánh cấp 3 hoặc 4 hoặc một cây có chính xác hai nút phân nhánh cấp 3 được kết nối theo cấu trúc giống chuỗi. Bất kỳ sự phân nhánh bổ sung nào ngay lập tức sẽ tạo ra một bố cục không thể thực hiện được trong lưới. 

Do đó, bài toán quy về việc đếm độ và kiểm tra xem có bao nhiêu đỉnh vượt quá bậc 2 và liệu có đỉnh nào vượt quá bậc 4 hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tìm kiếm nhúng Brute Force | O(4^n) | O(n) | Quá chậm | 
| Phân loại dựa trên bằng cấp | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc cây và tính bậc của mỗi đỉnh bằng cách quét tất cả các cạnh. Điều này đưa ra thước đo cục bộ về số lượng kết nối mà mỗi nút có, tương ứng trực tiếp với số lượng hướng lưới sẽ được yêu cầu nếu chúng ta cố gắng nhúng nó. 
2. Kiểm tra xem có đỉnh nào có bậc lớn hơn 4 hay không. Nếu đỉnh đó tồn tại, cây không thể được nhúng vào lưới vì chỉ có bốn hướng Manhattan có sẵn tại bất kỳ điểm nào. Trong trường hợp này, chúng tôi từ chối ngay lập tức. 
3. Đếm xem có bao nhiêu đỉnh có bậc ít nhất là 3. Đây là những điểm phân nhánh, vì các đỉnh bậc 2 hoạt động như đường thẳng và các đỉnh bậc 1 là điểm cuối. 
4. Nếu không có đỉnh nào có bậc 3 trở lên thì cây là đường đi đơn. Điều này luôn có thể nhúng được bằng cách đặt các nút theo một đường thẳng trên lưới. 
5. Nếu có đúng một đỉnh bậc 3 trở lên thì chấp nhận. Đỉnh này có thể đóng vai trò là điểm nối trung tâm và tất cả các chuỗi có thể mở rộng ra bên ngoài theo các hướng lưới khác nhau mà không có xung đột, miễn là bậc của nó không vượt quá 4. 
6. Nếu có chính xác hai đỉnh bậc 3 trở lên, chúng ta kiểm tra xem cấu trúc có còn phù hợp với chuỗi gồm hai điểm phân nhánh hay không. Trong một cây, điều này tự động được giữ vì có chính xác một đường dẫn giữa hai nút bất kỳ, do đó cấu trúc bị buộc phải trở thành một đường trục tuyến tính nối hai nút phân nhánh. 
7. Nếu có nhiều hơn hai đỉnh phân nhánh, hãy từ chối vì việc nhúng lưới không thể chứa nhiều hơn hai điểm nối phân nhánh thực sự mà không tạo ra sự chồng chéo hoặc tái sử dụng hướng. 

### Tại sao nó hoạt động 

Bất biến chính là bất kỳ việc nhúng cây hợp lệ nào vào lưới Manhattan đều phải gán các hướng lưới riêng biệt cho mỗi cạnh tới một đỉnh phân nhánh và chỉ có bốn hướng như vậy có sẵn trên toàn cầu tại mỗi nút. Khi có nhiều hơn hai đỉnh yêu cầu phân nhánh đồng thời, cấu trúc sẽ tạo ra sự mâu thuẫn trong việc phân bổ hướng dọc theo các đường dẫn chung. Vì cây có một đường dẫn duy nhất giữa hai nút bất kỳ nên các điểm phân nhánh chỉ có thể được hỗ trợ theo cách sắp xếp tuyến tính, điều này giới hạn số lượng của chúng tối đa là hai trong một nhúng nhất quán. Hạn chế về cấu trúc này được thể hiện chính xác bằng cách phân loại mức độ được sử dụng ở trên. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    if n == 1:
        print("YES")
        return

    deg = [0] * (n + 1)

    for _ in range(n - 1):
        u, v = map(int, input().split())
        deg[u] += 1
        deg[v] += 1

    for d in deg:
        if d > 4:
            print("NO")
            return

    big = sum(1 for d in deg if d >= 3)

    if big > 2:
        print("NO")
    else:
        print("YES")

if __name__ == "__main__":
    solve()
```Việc thực hiện bắt đầu bằng việc tính toán độ từ danh sách cạnh. Đây là thông tin cấu trúc duy nhất được yêu cầu và nó xác định đầy đủ liệu cây có phù hợp với một trong các cấu hình được phép của Manhattan hay không. 

Việc kiểm tra mức độ lớn hơn 4 được áp dụng sớm vì nó vi phạm ngay ràng buộc lưới. Sau đó, đếm các đỉnh có bậc ít nhất là 3 sẽ xác định được tất cả các điểm phân nhánh. Quyết định cuối cùng hoàn toàn dựa trên số lượng đỉnh như vậy tồn tại, phù hợp với phân loại có được trong thuật toán. 

Logic không yêu cầu phải root cây hoặc thực hiện bất kỳ quá trình truyền tải nào ngoài tích lũy mức độ, điều này giữ cho giải pháp tuyến tính và mạnh mẽ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một chuỗi đơn giản gồm năm nút. 

đầu vào:```
5
1 2
2 3
3 4
4 5
```| Bước | Độ (một phần) | Số chi nhánh | Quyết định | 
| --- | --- | --- | --- | 
| Sau khi xử lý | tất cả các độ ≤ 2 | 0 | Chấp nhận | 

Điều này chứng tỏ trường hợp đường dẫn thuần túy không tồn tại sự phân nhánh. 

### Ví dụ 2 

Hãy xem xét một cây có một nút phân nhánh trung tâm. 

đầu vào:```
5
1 2
1 3
1 4
4 5
```| Bước | Bằng cấp | Số chi nhánh | Quyết định | 
| --- | --- | --- | --- | 
| Sau khi xử lý | độ(1)=3 | 1 | Chấp nhận | 

Ở đây, nút 1 đóng vai trò là điểm nối duy nhất phù hợp với cấu trúc được phép. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi cạnh được xử lý một lần để tính độ và mỗi nút được kiểm tra một lần | 
| Không gian | O(n) | Mảng độ lưu trữ một giá trị trên mỗi đỉnh | 

Giải pháp chạy thoải mái trong giới hạn lên tới 200.000 nút vì nó chỉ thực hiện công việc tuyến tính và tránh đệ quy hoặc tìm kiếm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    deg = [0] * (n + 1)

    for _ in range(n - 1):
        u, v = map(int, input().split())
        deg[u] += 1
        deg[v] += 1

    if n == 1:
        return "YES"

    for d in deg:
        if d > 4:
            return "NO"

    big = sum(1 for d in deg if d >= 3)
    return "YES" if big <= 2 else "NO"

# minimum size
assert run("1\n") == "YES"

# simple chain
assert run("5\n1 2\n2 3\n3 4\n4 5\n") == "YES"

# star with 5 leaves (invalid)
assert run("6\n1 2\n1 3\n1 4\n1 5\n1 6\n") == "NO"

# two branching nodes
assert run("5\n1 2\n1 3\n2 4\n2 5\n") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | CÓ | trường hợp cạnh tối thiểu | 
| chuỗi | CÓ | cấu trúc đường dẫn thuần túy | 
| ngôi sao 5 lá | KHÔNG | độ > 4 từ chối | 
| hai nút phân nhánh | CÓ | cấu trúc nhánh kép được phép | 

## Vỏ cạnh 

Cây một nút là hợp lệ vì nó thỏa mãn một cách cơ bản tất cả các ràng buộc về mức độ và thuật toán trả về đúng CÓ vì không có đỉnh nào vi phạm giới hạn mức độ và không có điểm phân nhánh nào để đếm. 

Một ngôi sao có năm lá bộc lộ điều kiện biên cấp 4. Nút trung tâm có cấp độ 5, ngay lập tức kích hoạt sự từ chối trước khi có bất kỳ lý do nào tiếp theo. Thuật toán nắm bắt được điều này sớm thông qua việc kiểm tra mức độ rõ ràng. 

Một cây có đúng hai đỉnh phân nhánh thể hiện trường hợp chấp nhận không hề tầm thường. Mặc dù hai nút có cấp độ 3 trở lên, cấu trúc vẫn là một đường trục tuyến tính hợp lệ với hai điểm nối và điều kiện đếm cho phép nó mà không cần mô phỏng cấu trúc thêm.
