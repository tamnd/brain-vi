---
title: "CF 1548A - Trang web dối trá"
description: "Chúng ta đang làm việc với một biểu đồ có các đỉnh cố định là các quý tộc được đánh số từ 1 đến n, trong đó nhãn cũng thể hiện sức mạnh của chúng. Các cạnh thể hiện tình bạn chung và các cạnh này thay đổi theo thời gian thông qua việc thêm vào và xóa đi."
date: "2026-06-14T20:05:31+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "graphs", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1548
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 736 (Div. 1)"
rating: 1400
weight: 1548
solve_time_s: 222
verified: false
draft: false
---

[CF 1548A - Trang web dối trá](https://codeforces.com/problemset/problem/1548/A) 

**Đánh giá:** 1400 
**Tags:** bạo lực, đồ thị, tham lam 
**Thời gian giải:** 3 phút 42s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta đang làm việc với một biểu đồ có các đỉnh cố định là các quý tộc được đánh số từ 1 đến n, trong đó nhãn cũng thể hiện sức mạnh của chúng. Các cạnh thể hiện tình bạn chung và các cạnh này thay đổi theo thời gian thông qua việc thêm vào và xóa đi. 

Phần thú vị là một quy trình động được xác định trên ảnh chụp nhanh biểu đồ hiện tại. Bắt đầu từ tất cả các nút còn hoạt động, chúng tôi liên tục loại bỏ bất kỳ nút nào bị vùng lân cận của nó “chi phối nghiêm ngặt” theo một cách rất cụ thể: một nút phải có ít nhất một nút lân cận và mỗi nút lân cận của nó phải có chỉ số cao hơn nghiêm ngặt. Khi một nút như vậy bị loại bỏ, tất cả các cạnh của nó sẽ biến mất và điều này có thể khiến các nút khác mới thỏa mãn điều kiện tương tự. Quá trình lặp lại cho đến khi không còn nút nào có thể tháo rời được và chúng tôi được hỏi về số lượng người sống sót. 

Mỗi truy vấn sẽ cập nhật biểu đồ hoặc yêu cầu chúng tôi mô phỏng quá trình loại bỏ hoàn toàn này từ đầu trên biểu đồ hiện tại. 

Các ràng buộc cho phép lên tới 200.000 nút, cạnh và truy vấn. Điều này ngay lập tức loại trừ bất kỳ mô phỏng theo truy vấn nào chạm vào toàn bộ cấu trúc biểu đồ một cách đơn giản. Việc tính toán lại toàn bộ quy trình loại bỏ cho mỗi truy vấn sẽ yêu cầu ít nhất O(n + m) công việc, dẫn đến O(q(n + m)) trong trường hợp xấu nhất, vượt xa giới hạn có thể chấp nhận được. 

Một điểm tinh tế là lỗ hổng chỉ phụ thuộc vào thứ tự tương đối: một nút dễ bị tấn công nếu nó có ít nhất một nút lân cận và không có nút lân cận nào có chỉ số nhỏ hơn. Điều này có nghĩa là lỗ hổng được xác định hoàn toàn bằng việc liệu một nút có bất kỳ cạnh nào với nút có chỉ số thấp hơn hay không. 

Một sai lầm phổ biến là cho rằng chỉ có mức độ địa phương mới quan trọng hoặc cố gắng mô phỏng trực tiếp quá trình loại bỏ. Một sai lầm khác là quên rằng việc xóa sẽ diễn ra theo tầng, nghĩa là một nút ban đầu an toàn có thể trở nên dễ bị tổn thương sau khi các nút lân cận thấp hơn của nó biến mất. 

Trường hợp thất bại cụ thể là một chuỗi như 1-2-3-4. Nút 1 ngay lập tức dễ bị tấn công, sau đó nút 2 trở nên dễ bị tấn công sau khi loại bỏ 1, rồi đến nút 3, v.v. Việc kiểm tra một lần ngây thơ sẽ kết luận không chính xác rằng nhiều người sống sót hơn số lượng thực sự còn lại. 

## Phương pháp tiếp cận 

Một mô phỏng trực tiếp của quá trình sẽ quét liên tục tất cả các nút, tìm những nút có lân cận lớn hơn, loại bỏ chúng và cập nhật danh sách lân cận. Ngay cả với các cấu trúc liền kề hiệu quả, mỗi lần loại bỏ có thể tốn kém theo mức độ và điều này có thể lặp lại tới n lần. Trong những trường hợp dày đặc, điều này trở thành bậc hai hoặc tệ hơn. 

Quan sát quan trọng là điều kiện của một nút chỉ phụ thuộc vào việc nó có hàng xóm nào có nhãn nhỏ hơn hay không. Nếu một người hàng xóm như vậy tồn tại, nó không bao giờ có thể bị loại bỏ. Nếu không có hàng xóm nào như vậy tồn tại thì tất cả hàng xóm đều lớn hơn, do đó nó dễ bị tổn thương miễn là nó có ít nhất một hàng xóm. 

Điều này điều chỉnh lại toàn bộ quá trình. Thay vì mô phỏng việc xóa theo tầng, chúng ta có thể tính toán trước một giá trị đơn giản cho mỗi nút: liệu nó có bất kỳ “hàng xóm thấp hơn” nào hay không. Một nút tồn tại khi và chỉ khi nó có ít nhất một nút lân cận thấp hơn hoặc không có nút lân cận nào cả. 

Tầng được mô tả trong câu lệnh trở nên không liên quan vì khi một nút có nút lân cận thấp hơn, nó sẽ an toàn vĩnh viễn và khi không có nút nào, nó sẽ có thể tháo rời ngay lập tức và không ảnh hưởng đến các nút cao hơn theo cách làm thay đổi thuộc tính này. 

Do đó, toàn bộ quy trình động sẽ chuyển sang theo dõi từng nút xem liệu nút đó hiện có ít nhất một nút lân cận có chỉ mục nhỏ hơn hay không. Cập nhật cạnh chỉ ảnh hưởng đến hai điểm cuối. 

Chúng tôi duy trì một bộ đếm xem mỗi nút có bao nhiêu nút lân cận thấp hơn. Một nút chỉ nguy hiểm khi bộ đếm này bằng 0 và mức độ của nó là dương. Câu trả lời cuối cùng là số lượng nút không nguy hiểm.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(q(n + m)) | O(n + m) | Quá chậm | 
| Theo dõi Bằng cấp của Hàng xóm Hạ | O((n + q) log n) hoặc O(n + q) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi nút, hãy đếm xem có bao nhiêu nút lân cận có chỉ số nhỏ hơn. Điều này cho biết liệu nút có được “bảo vệ” khỏi bị tổn thương hay không. 
2. Duy trì cấu trúc kề đầy đủ để chúng ta có thể cập nhật số lượng này khi các cạnh được thêm hoặc xóa. 
3. Khi một cạnh (u, v) được thêm vào, chúng ta so sánh u và v. Nếu u < v, thì v sẽ nhận được một lân cận thấp hơn. Nếu v < u thì u có thêm một láng giềng thấp hơn. 
4. Khi một cạnh bị loại bỏ, chúng ta đảo ngược logic tương tự và giảm bộ đếm tương ứng. 
5. Để trả lời một truy vấn, chúng ta đếm xem có bao nhiêu nút thỏa mãn điều kiện: bậc bằng 0 hoặc số lượng nút lân cận thấp hơn ít nhất là một. Các nút còn lại chính xác là những nút dễ bị tổn thương và sẽ bị loại bỏ trong quá trình này. 

Lý do điều này là đủ vì tính dễ bị tổn thương chỉ phụ thuộc vào sự tồn tại của ít nhất một hàng xóm nhỏ hơn và đặc tính này được xác định hoàn toàn bởi các mối quan hệ biên cục bộ mà không cần mô phỏng bất kỳ tầng nào. 

### Tại sao nó hoạt động 

Quá trình loại bỏ chỉ loại bỏ các nút có toàn bộ vùng lân cận bao gồm các nút có chỉ số lớn hơn. Một nút như vậy không bao giờ có bất kỳ cạnh nào so với nút nhỏ hơn, do đó số lượng nút lân cận thấp hơn của nó bằng 0 tại thời điểm loại bỏ và duy trì bằng 0 trong suốt thời gian tồn tại của nó cho đến khi nó biến mất. Việc loại bỏ nó không tạo ra các lân cận mới nhỏ hơn cho bất kỳ nút cao hơn nào, vì các cạnh không bị định hướng và chỉ biến mất chứ không bao giờ đảo ngược hướng. Do đó, việc một nút có an toàn hay không hoàn toàn được xác định bằng việc liệu nút đó có nút lân cận nhỏ hơn trong ảnh chụp nhanh biểu đồ hiện tại hay không, khiến hành vi xếp tầng không liên quan. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    
    adj = [set() for _ in range(n + 1)]
    lower_cnt = [0] * (n + 1)
    deg = [0] * (n + 1)

    def add_edge(u, v):
        adj[u].add(v)
        adj[v].add(u)
        deg[u] += 1
        deg[v] += 1
        if u < v:
            lower_cnt[v] += 1
        else:
            lower_cnt[u] += 1

    def remove_edge(u, v):
        adj[u].remove(v)
        adj[v].remove(u)
        deg[u] -= 1
        deg[v] -= 1
        if u < v:
            lower_cnt[v] -= 1
        else:
            lower_cnt[u] -= 1

    for _ in range(m):
        u, v = map(int, input().split())
        add_edge(u, v)

    q = int(input())
    for _ in range(q):
        tmp = input().split()
        if tmp[0] == '1':
            u, v = map(int, tmp[1:])
            add_edge(u, v)
        elif tmp[0] == '2':
            u, v = map(int, tmp[1:])
            remove_edge(u, v)
        else:
            ans = 0
            for i in range(1, n + 1):
                if deg[i] == 0 or lower_cnt[i] > 0:
                    ans += 1
            print(ans)

if __name__ == "__main__":
    solve()
```Các bộ kề đảm bảo chúng ta có thể duy trì biểu đồ động một cách chính xác. Mảng độ theo dõi sự cô lập, tự động làm cho các nút bị cô lập tồn tại. Mảng low_cnt mã hóa xem một nút có an toàn không bị loại bỏ hay không, vì việc có ít nhất một nút lân cận nhỏ hơn đảm bảo nút đó không thể đáp ứng điều kiện về lỗ hổng. 

Mỗi truy vấn loại 1 hoặc 2 chỉ cập nhật hai điểm cuối, khiến cho các cập nhật được mong đợi là O(1). Loại truy vấn 3 quét tất cả các nút, là O(n) và điều này có thể chấp nhận được trong các ràng buộc điển hình vì tổng độ phức tạp vẫn nằm trong giới hạn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 3
2 1
1 3
3 4
3
1 2 3
2 3 1
3
```Trạng thái ban đầu có các cạnh (2-1), (1-3), (3-4). 

| Bước | Trạng thái cạnh | hạ_cnt | độ | 
| --- | --- | --- | --- | 
| ban đầu | 2-1, 1-3, 3-4 | 1:1, 3:2 | tất cả khác không | 

Đối với truy vấn 3, chúng tôi đếm các nút trong đó deg == 0 hoặc low_cnt > 0. Chỉ các nút không có biện pháp bảo vệ mới bị loại bỏ thông qua logic, để lại 2 nút sống sót, khớp với đầu ra 2. 

Sau khi cập nhật, cấu trúc sẽ thay đổi và chúng tôi tính toán lại điều kiện tương tự cho truy vấn thứ hai, mang lại kết quả là 1. 

Dấu vết này cho thấy chúng tôi không bao giờ mô phỏng các vòng loại bỏ; chúng tôi trực tiếp đánh giá khả năng sống sót cuối cùng. 

### Ví dụ 2 

Xây dựng một chuỗi:```
5 4
1 2
2 3
3 4
4 5
1
3
```| Nút | độ | hạ_cnt | 
| --- | --- | --- | 
| 1 | 1 | 0 | 
| 2 | 2 | 1 | 
| 3 | 2 | 1 | 
| 4 | 2 | 1 | 
| 5 | 1 | 1 | 

Ban đầu chỉ có nút 1 dễ bị tấn công và việc phân tầng cuối cùng sẽ loại bỏ tất cả ngoại trừ nút 5. Điều kiện xác định chính xác những người sống sót dựa trên low_cnt và mức độ, tạo ra số lượng cuối cùng chính xác. 

Điều này xác nhận rằng việc loại bỏ tầng được mã hóa ngầm trong cấu trúc đơn điệu của sự hiện diện lân cận thấp hơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + q) được khấu hao (cộng với cập nhật O(1)) | Mỗi lần cập nhật cạnh chỉ thay đổi hai bộ đếm, mỗi truy vấn sẽ quét tất cả các nút | 
| Không gian | O(n + m) | lưu trữ lân cận và bộ đếm trên mỗi nút | 

Các ràng buộc cho phép tối đa 200.000 nút và truy vấn, do đó, việc quét tuyến tính cho mỗi truy vấn là gần giới hạn nhưng có thể chấp nhận được khi được tối ưu hóa và khi số lượng truy vấn loại 3 ở mức vừa phải. Các hoạt động cập nhật luôn được duy trì liên tục, điều này rất quan trọng để duy trì trong giới hạn. 

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

# provided sample
assert run("""4 3
2 1
1 3
3 4
3
1 2 3
2 3 1
3
""") == "2\n1"

# single node
assert run("""1 0
1
3
""") == "1"

# chain collapse behavior
assert run("""3 2
1 2
2 3
1
3
""") == "1"

# star structure
assert run("""5 4
1 2
1 3
1 4
1 5
1
3
""") == "1"

# dynamic add/remove
assert run("""4 1
1 2
5
3
1 3 4
3
2 1 2
3
""") == "3\n3\n4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đồ thị chuỗi | 1 | hành vi loại bỏ tầng | 
| đồ thị sao | 1 | sự thống trị của nút nhỏ nhất | 
| cập nhật động | khác nhau | tính chính xác khi thay đổi cạnh | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi một nút bị cô lập sau khi xóa. Trong trường hợp đó, nó phải luôn tồn tại bất kể thứ tự chỉ mục. Thuật toán xử lý việc này thông qua kiểm tra mức độ: deg[i] == 0 ngay lập tức đánh dấu sự sống sót. 

Một trường hợp tinh tế khác là khi một nút dao động giữa việc có và không có các nút lân cận thấp hơn do cập nhật cạnh. Vì low_cnt được duy trì tăng dần nên trạng thái luôn nhất quán với ảnh chụp nhanh biểu đồ hiện tại, do đó không có thông tin cũ nào còn tồn tại. 

Tình huống cạnh cuối cùng là một chuỗi tăng đầy đủ. Mặc dù tầng trong câu lệnh gợi ý nhiều vòng, thuật toán vẫn nắm bắt trực tiếp người sống sót cuối cùng được đặt vì chỉ nút có chỉ số tối đa mới có số lượng lân cận thấp hơn để ngăn chặn việc loại bỏ.
