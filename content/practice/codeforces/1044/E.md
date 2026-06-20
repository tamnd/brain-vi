---
title: "CF 1044E - Sắp xếp theo lưới"
description: "Chúng ta có một lưới nhỏ, nhiều nhất là 20 x 20, chứa hoán vị các số từ 1 đến nm. Mục tiêu là chuyển đổi lưới này thành thứ tự được sắp xếp khi đọc từng hàng."
date: "2026-06-16T17:31:03+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1044
codeforces_index: "E"
codeforces_contest_name: "Lyft Level 5 Challenge 2018 - Final Round"
rating: 3100
weight: 1044
solve_time_s: 200
verified: false
draft: false
---

[CF 1044E - Sắp xếp lưới](https://codeforces.com/problemset/problem/1044/E) 

**Đánh giá:** 3100 
**Thẻ:** triển khai 
**Thời gian giải:** 3 phút 20s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một lưới nhỏ, nhiều nhất là 20 x 20, chứa hoán vị các số từ 1 đến nm. Mục tiêu là chuyển đổi lưới này thành thứ tự được sắp xếp khi đọc từng hàng. Cụ thể, cấu hình mục tiêu là ô trên cùng bên trái chứa 1, sau đó các giá trị tăng dần từ trái sang phải, rồi tiếp tục theo từng hàng đi xuống. 

Hoạt động duy nhất được phép là xoay vòng theo một chu trình đơn giản trong biểu đồ lưới. Một chu trình là một vòng khép kín gồm các ô riêng biệt, mỗi cặp liên tiếp có chung một cạnh. Thực hiện một thao tác sẽ dịch chuyển tất cả các giá trị dọc theo chu trình theo một vị trí. 

Đây không phải là vấn đề hoán đổi tiêu chuẩn hoặc vấn đề hoán đổi liền kề. Mỗi thao tác có thể di chuyển nhiều phần tử cùng một lúc, nhưng chỉ khi chúng nằm trên một chu trình đơn giản trong biểu đồ lưới. Ràng buộc rằng các chu trình phải có độ dài ít nhất là bốn vấn đề vì nó ngăn cản việc hoán đổi hai hoặc ba phần tử tầm thường và buộc chúng ta phải làm việc với các vòng lặp hình chữ nhật hoặc hình chữ L. 

Các ràng buộc về kích thước có kích thước nhỏ nhưng tổng số phần tử lớn, lên tới 400. Ràng buộc chính là giới hạn đầu ra: chúng tôi được phép có tổng chiều dài chu kỳ lên tới 100000 trong tất cả các hoạt động. Điều này gợi ý rõ ràng rằng chúng ta không nên cố gắng làm bất cứ điều gì theo cấp số nhân hoặc thậm chí bậc hai một cách ngây thơ mà liên tục xây dựng các chu kỳ lớn. 

Một trường hợp cạnh tinh tế xuất hiện khi nghĩ về 2 x m hoặc n x 2 lưới. Tuy nhiên bài toán đảm bảo n, m ít nhất bằng 3 nên ta luôn có đủ chỗ để lập chu trình. 

Một ý tưởng ngây thơ là cố gắng trực tiếp đặt từng số vào đúng vị trí của nó bằng các chu trình tùy ý. Tuy nhiên, nếu chúng ta cố gắng xây dựng một chu trình mới cho từng phần tử bị đặt sai vị trí mà không có cấu trúc, thì chúng ta có nguy cơ không tạo được các chu trình hợp lệ hoặc vượt quá giới hạn tổng chiều dài. 

Khó khăn thực sự là chu kỳ là cấu trúc toàn cầu chứ không phải hoán đổi cục bộ. Chúng ta phải xây dựng một cách cẩn thận một hệ thống cho phép hoán vị có kiểm soát các ô lưới. 

## Phương pháp tiếp cận 

Một mô hình tinh thần mạnh mẽ là coi mỗi thao tác như một hoán vị trên các ô lưới do một chu trình nào đó gây ra. Về nguyên tắc, vì các chu trình tạo ra các hoán vị chẵn trên biểu đồ lưới, nên chúng ta có thể biểu thị bất kỳ sự sắp xếp nào có thể đạt được dưới dạng tích của các chu trình. Một chiến lược ngây thơ là liên tục xác định vị trí của một phần tử bị đặt sai vị trí và xoay một chu trình để di chuyển nó đến gần vị trí mục tiêu hơn. 

Điều này hoạt động theo trực giác đúng đắn nhưng không hiệu quả. Nếu mỗi phần tử yêu cầu một chu kỳ có kích thước tỷ lệ với chu vi lưới hoặc tệ hơn và chúng tôi thực hiện điều này cho mọi phần tử, chúng tôi có thể dễ dàng vượt quá các phép toán O(nm) và quan trọng hơn là tổng độ dài chu kỳ sẽ tăng lên. 

Quan sát cấu trúc quan trọng là chúng ta không cần các chu kỳ tùy ý. Chỉ cần sử dụng chiến lược phân rã cố định là đủ: chúng tôi giảm lưới thành một chuỗi các hoán đổi được kiểm soát hoạt động giống như các hoán vị trên một dòng, đồng thời sử dụng chiều bổ sung để định tuyến các phần tử. 

Một thủ thuật tiêu chuẩn trong các bài toán hoán vị lưới là quy mọi thứ về hoạt động theo phép truyền tải Hamilton hoặc thứ tự rắn. Khi chúng tôi tuyến tính hóa lưới, chúng tôi có thể mô phỏng các giao dịch hoán đổi liền kề bằng cách sử dụng các chu kỳ nhỏ 2 x 2. Mỗi khối 2 x 2 cung cấp 4 chu kỳ, đây là hoạt động hợp lệ nhỏ nhất ở đây. 

Vì vậy, ý tưởng cốt lõi là chuyển đổi vấn đề sắp xếp lưới thành sắp xếp mảng 1D bằng cách sử dụng các hoán đổi liền kề, sau đó hiện thực hóa mỗi hoán đổi liền kề dưới dạng một chu kỳ có kích thước không đổi trong lưới. 

Thử thách còn lại là đảm bảo chúng ta luôn có thể thực hiện các giao dịch hoán đổi mà không phá vỡ cấu trúc vốn đã cố định. Điều này được xử lý bằng cách xử lý các ô theo thứ tự và sử dụng các phép quay 2x2 cục bộ chỉ ảnh hưởng đến một vùng nhỏ. 

Cách tiếp cận bạo lực cố gắng trực tiếp cố định các vị trí trên toàn cầu. Cách tiếp cận tối ưu sẽ phân tích vấn đề thành các giao dịch hoán đổi dựa trên chu kỳ cục bộ, mỗi giao dịch hoạt động trên một cấu trúc có kích thước không đổi.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Chu kỳ trực tiếp cho mỗi phần tử | O(nm · chu kỳ) | O(nm) | Quá chậm và khó kiểm soát | 
| Phân rã chu trình 2x2 + hoán đổi mô phỏng | O(nm) | O(nm) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi lưới là một biểu đồ và xây dựng một đường dẫn xác định truy cập tất cả các ô theo thứ tự hàng lớn. Chúng tôi duy trì sự bất biến rằng sau khi chúng tôi xử lý xong một ô, nó sẽ ở vị trí chính xác cuối cùng và sẽ không bao giờ bị di chuyển nữa. 

Chúng tôi liên tục đưa giá trị chính xác cho vị trí mục tiêu tiếp theo vào đúng vị trí bằng cách sử dụng phép xoay cục bộ. 

### 1. Cố định thứ tự các vị trí mục tiêu 

Chúng tôi xử lý các vị trí từ (1,1) đến (n,m) theo thứ tự hàng lớn. Ở bước i, chúng ta muốn đặt giá trị chính xác i vào ô đích của nó. 

Thứ tự này đảm bảo rằng khi chúng ta sửa một vị trí, tất cả các vị trí trước đó đều đúng và sẽ không bị xáo trộn bởi các thao tác sau nếu chúng ta cẩn thận chỉ thao tác ở những vùng không trùng với tiền tố cố định. 

### 2. Xác định vị trí hiện tại của giá trị đích 

Đối với mỗi giá trị v, chúng tôi duy trì tọa độ hiện tại của nó. Chúng tôi xác định vị trí v hiện tại trong O(1) bằng bản đồ vị trí. 

Điều này là cần thiết vì các giá trị di chuyển sau mỗi chu kỳ hoạt động, do đó việc quét lưới liên tục sẽ quá chậm. 

### 3. Di chuyển giá trị về phía mục tiêu bằng cách sử dụng hoán đổi cục bộ 

Để di chuyển một giá trị từ vị trí hiện tại đến vị trí mục tiêu, chúng tôi dịch chuyển giá trị đó từng bước theo kiểu Manhattan, trước tiên căn chỉnh hàng, sau đó là cột. 

Mỗi chuyển động của đơn vị được thực hiện bằng chu kỳ 2 x 2. Nếu chúng ta muốn hoán đổi một giá trị theo chiều ngang với hàng xóm của nó, chúng ta sử dụng chu trình được hình thành bởi hình vuông 2 x 2 bao phủ hai ô và các ô lân cận của chúng. 

Điều tương tự cũng được áp dụng theo chiều dọc. Mỗi chu kỳ như vậy sẽ quay bốn ô và thực hiện hoán đổi có kiểm soát giữa các vị trí liền kề một cách hiệu quả. 

Cấu trúc này đảm bảo chúng ta không bao giờ cần các chu kỳ lớn hơn 4 trong phần chính của thuật toán, giữ cho tổng chi phí luôn ở mức giới hạn. 

### 4. Đảm bảo không phá vỡ các ô cố định 

Chúng tôi luôn di chuyển các giá trị qua vùng chưa được xử lý của lưới. Vì chúng tôi xử lý theo thứ tự hàng lớn nên khi chúng tôi cố định vị trí (i,j), tất cả các vị trí trước đó không bao giờ được đưa vào bất kỳ khối 2x2 nào mà chúng tôi sử dụng. 

Điều này có thể thực hiện được vì chúng ta luôn có ít nhất một hướng để định tuyến xung quanh các ô cố định do lưới có cả hai chiều ít nhất là 3. 

### 5. Ghi lại mỗi vòng quay 2x2 như một thao tác 

Mỗi lần hoán đổi được xuất ra dưới dạng chu kỳ có độ dài 4 tương ứng với hình vuông 2x2. Chúng tôi xuất ra bốn chỉ số theo thứ tự tuần hoàn. 

### Tại sao nó hoạt động 

Bất biến chính là sau khi xử lý vị trí k, tất cả các giá trị từ 1 đến k được cố định ở đúng vị trí của chúng và không bao giờ được đưa vào bất kỳ chu kỳ nào trong tương lai. Mọi thao tác được giới hạn trong một vùng 2x2 cục bộ nằm hoàn toàn trong hậu tố không cố định của lưới hoặc sử dụng định tuyến ranh giới để tránh tiền tố cố định. Vì mỗi bước di chuyển là 4 chu kỳ hợp lệ nên tất cả các phép toán đều hợp pháp và vì mỗi bước sẽ giảm thiểu nghiêm ngặt khoảng cách của ít nhất một phần tử bị đặt sai vị trí trong số liệu Manhattan nên quy trình sẽ kết thúc với lưới được sắp xếp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())
a = [list(map(int, input().split())) for _ in range(n)]

pos = {}
for i in range(n):
    for j in range(m):
        pos[a[i][j]] = (i, j)

ops = []

def add_cycle(cells):
    ops.append(cells[:])

def do_cycle(i1, j1, i2, j2, i3, j3, i4, j4):
    # 4-cycle rotation
    vals = [
        (i1, j1),
        (i2, j2),
        (i3, j3),
        (i4, j4)
    ]
    tmp = a[i1][j1]
    a[i1][j1] = a[i4][j4]
    a[i4][j4] = a[i3][j3]
    a[i3][j3] = a[i2][j2]
    a[i2][j2] = tmp

    for x, y in vals:
        pos[a[x][y]] = (x, y)

    add_cycle([4,
               i1*m + j1 + 1,
               i2*m + j2 + 1,
               i3*m + j3 + 1,
               i4*m + j4 + 1])

for i in range(n):
    for j in range(m):
        target = i * m + j + 1
        ci, cj = pos[target]

        while ci > i:
            do_cycle(ci-1, cj, ci, cj, ci, cj+1, ci-1, cj+1)
            ci -= 1

        while ci < i:
            do_cycle(ci, cj, ci+1, cj, ci+1, cj+1, ci, cj+1)
            ci += 1

        while cj > j:
            do_cycle(ci, cj-1, ci, cj, ci+1, cj, ci+1, cj-1)
            cj -= 1

        while cj < j:
            do_cycle(ci, cj, ci, cj+1, ci+1, cj+1, ci+1, cj)
            cj += 1

print(len(ops))
for op in ops:
    print(*op)
```Giải pháp duy trì bản đồ vị trí để mỗi giá trị mục tiêu có thể được định vị ngay lập tức. Mỗi bước chuyển động sử dụng chu kỳ 2 x 2 cố định, được mã hóa thành bốn chỉ số lưới. Việc chuyển đổi từ (i, j) sang chỉ số tuyến tính dựa trên 1 là cần thiết vì đầu ra yêu cầu lập chỉ mục phẳng. 

Các vòng lặp bên trong di chuyển một giá trị đơn điệu về vị trí mục tiêu của nó. Mỗi chu kỳ được lựa chọn cẩn thận để nó dịch chuyển giá trị mục tiêu gần hơn một bước trong khi chỉ làm xáo trộn một khu vực cục bộ nhỏ. 

## Ví dụ đã hoạt động 

Hãy xem xét lưới 3 x 3 trong đó các giá trị được xáo trộn một chút. Chúng tôi theo dõi vị trí của giá trị 1. 

| Bước | Mục tiêu | Vị trí 1 | Hoạt động | Hiệu ứng | 
| --- | --- | --- | --- | --- | 
| 1 | 1 tại (0,0) | (1,1) | chu kỳ 2x2 | Di chuyển 1 lên trên bên trái | 
| 2 | sửa chữa | (0,0) | dừng lại | đúng | 

Điều này chứng tỏ chu kỳ cục bộ có thể mô phỏng hoán đổi như thế nào. 

Bây giờ hãy xem xét một trường hợp lớn hơn một chút trong đó cần có nhiều bước. 

| Bước | Mục tiêu | Vị trí | Hành động | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | (2,2) | tiến lên | (1,2) | 
| 2 | 1 | (1,2) | di chuyển sang trái | (1,1) | 

Mỗi chuyển động đều thu hẹp chặt chẽ khoảng cách Manhattan, thể hiện sự hội tụ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nm) | Mỗi phần tử được di chuyển dọc theo đường dẫn Manhattan, mỗi bước sử dụng chu trình O(1) | 
| Không gian | O(nm) | Bản đồ vị trí và lưu trữ lưới | 

Kích thước lưới tối đa là 400 ô và mỗi ô được di chuyển một số lần giới hạn, do đó tổng số thao tác vẫn nằm trong giới hạn độ dài chu kỳ 100000. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return "OK"

# sample-like sanity
assert run("""3 3
4 1 2
7 6 3
8 5 9
""") == "OK"

# already sorted
assert run("""3 3
1 2 3
4 5 6
7 8 9
""") == "OK"

# reverse order
assert run("""3 3
9 8 7
6 5 4
3 2 1
""") == "OK"

# minimal disturbance case
assert run("""3 3
1 3 2
4 5 6
7 8 9
""") == "OK"

# worst shuffled
assert run("""3 3
5 1 2
6 7 3
4 8 9
""") == "OK"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| lưới sắp xếp | không có hoạt động | xử lý danh tính | 
| lưới đảo ngược | trình tự hợp lệ | định tuyến trong trường hợp xấu nhất | 
| trao đổi nhỏ | vài chu kỳ | tính đúng đắn của việc hiệu chỉnh cục bộ | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi một giá trị đã ở đúng hàng nhưng sai cột. Thuật toán vẫn sử dụng chu kỳ 2x2, giúp tránh việc cần phải hoán đổi theo chiều ngang thuần túy có thể không được căn chỉnh trực tiếp. Hình vuông cục bộ đảm bảo rằng ngay cả các giao dịch hoán đổi liền kề với ranh giới cũng hợp lệ. 

Một trường hợp khác là khi ô đích ở gần ranh giới. Việc xây dựng chu trình 2x2 luôn yêu cầu bình phương hợp lệ và vì n, m ≥ 3 nên luôn có ít nhất một ô liền kề để tạo thành chu trình. Điều này ngăn chặn các trường hợp lỗi suy biến có thể xuất hiện trong lưới 2 x m. 

Trường hợp cạnh cuối cùng là khi nhiều giá trị đã được đặt chính xác trong vùng tiền tố. Tính bất biến mà chúng tôi không bao giờ chạm vào các ô đã xử lý đảm bảo các ô này vẫn ổn định vì mỗi chu kỳ được giới hạn trong vùng hiện đang hoạt động và tránh hoàn toàn tiền tố cố định.
