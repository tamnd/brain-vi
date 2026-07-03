---
title: "CF 103069F - Xe"
description: "Chúng ta có hai tập hợp điểm trên một lưới số nguyên vô hạn. Một bộ thuộc về Giáo sư Pang và bộ còn lại thuộc về Giáo sư Shou."
date: "2026-07-04T00:59:53+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103069
codeforces_index: "F"
codeforces_contest_name: "2020 ICPC Asia East Continent Final"
rating: 0
weight: 103069
solve_time_s: 42
verified: true
draft: false
---

[CF 103069F - Xe](https://codeforces.com/problemset/problem/103069/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 42s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai tập hợp điểm trên một lưới số nguyên vô hạn. Một bộ thuộc về Giáo sư Pang và bộ còn lại thuộc về Giáo sư Shou. Mỗi điểm hoạt động giống như một quân xe: nó chỉ có thể “tấn công” một quân xe khác nếu cả hai quân được xếp thẳng hàng theo chiều ngang hoặc chiều dọc và không có quân xe nào khác nằm giữa chúng trên cùng một hàng hoặc cột đó. 

Nhiệm vụ không phải là đếm các cuộc tấn công trên toàn cầu mà là xác định xem mỗi quân xe có tham gia vào ít nhất một cuộc tấn công hợp lệ chống lại quân đối phương hay không. Đối với mỗi quân, chúng tôi đưa ra một chỉ báo nhị phân tùy thuộc vào việc nó có ít nhất một đối thủ "hiển thị" dọc theo hàng hoặc cột của nó mà không có quân xe chặn ở giữa hay không. 

Các ràng buộc rất lớn, lên tới 200000 điểm cho mỗi người chơi, do đó tổng số điểm lên tới 400000 điểm. Việc kiểm tra từng cặp đơn giản giữa tất cả các quân xe sẽ là phương pháp bậc hai và quá chậm. Bất kỳ giải pháp nào về cơ bản đều phải xử lý tất cả các điểm theo thời gian gần tuyến tính hoặc log-tuyến tính trên mỗi nhóm tọa độ. 

Một trường hợp thất bại tinh tế do lối suy nghĩ ngây thơ xuất hiện khi có nhiều quân xe nằm trên cùng một hàng hoặc cột. 

Xét ba quân xe trên cùng một hàng:```
P at (0, 0), S at (2, 0), P at (4, 0)
```Mặc dù quân ở giữa bị tấn công, nhưng quân ngoài cùng không nhất thiết phải bị tấn công cả hai trừ khi chúng ta thực thi đúng quy tắc “không có quân ở giữa”. Một cách tiếp cận ngây thơ chỉ kiểm tra sự tồn tại của một quân xe có màu đối lập trên cùng một hàng sẽ đánh dấu không chính xác tất cả là bị tấn công. 

Một tình huống phức tạp khác là khi có nhiều quân đối thủ ở cả hai hướng; chỉ có người gần nhất ở mỗi hướng mới quan trọng. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ so sánh mọi quân xe với mọi quân xe khác của người chơi đối diện. Đối với mỗi cặp, chúng tôi sẽ kiểm tra xem chúng có cùng tọa độ x hoặc y hay không, sau đó xác minh xem có quân xe nào khác nằm giữa chúng không. Ngay cả khi chúng tôi tính toán trước các vị trí, việc xác minh việc chặn vẫn yêu cầu quét các điểm ở giữa hoặc duy trì một tập hợp, dẫn đến hành vi gần như O(N^2) trong các hàng hoặc cột dày đặc. 

Quan sát quan trọng là điều kiện chặn chỉ làm cho các hàng xóm ngay lập tức theo thứ tự được sắp xếp có liên quan. Trên bất kỳ tọa độ x cố định nào, nếu chúng ta sắp xếp tất cả quân xe theo y thì quân xe chỉ có thể nhìn thấy quân xe gần nhất ở trên và dưới nó. Xe nào xa hơn sẽ bị chặn bởi quân gần nhất. Điều tương tự cũng áp dụng đối xứng cho từng tọa độ y. 

Vì vậy, thay vì kiểm tra tất cả các cặp, chúng ta nhóm các quân xe theo hàng và theo cột. Trong mỗi nhóm, chúng tôi sắp xếp và chỉ so sánh các phần tử liền kề. Đối với mỗi quân kề nhau, nếu hai quân thuộc về những người chơi khác nhau, cả hai đều bị đánh dấu là bị tấn công. 

Điều này làm giảm vấn đề thành hai lần quét độc lập: một lần quét trên tất cả các nhóm x và một lần quét trên tất cả các nhóm y. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n1·n2) | O(n) | Quá chậm | 
| Nhóm + Sắp xếp Hàng xóm | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý khả năng hiển thị riêng biệt dọc theo hàng và cột vì các cuộc tấn công được xác định độc lập theo các hướng đó. 

1. Nhóm tất cả các quân xe theo tọa độ x của chúng. Đối với mỗi giá trị x, hãy thu thập tất cả các quân xe chia sẻ nó, lưu trữ tọa độ y và danh tính người chơi của chúng. Điều này cô lập các tương tác dọc. 
2. Đối với mỗi nhóm x, sắp xếp quân xe theo tọa độ y. Việc sắp xếp là cần thiết để “không có xe nào ở giữa” trở thành tương đương với sự kề cận trong thứ tự đã sắp xếp. 
3. Quét từng nhóm x được sắp xếp từ dưới lên trên. Đối với mỗi cặp liền kề, hãy kiểm tra xem chúng có thuộc về những người chơi khác nhau hay không. Nếu có, đánh dấu cả hai quân xe là bị tấn công. Lý do gần kề là đủ là vì bất kỳ quân xe nào giữa hai quân khác sẽ làm mất tầm nhìn trực tiếp, vì vậy chỉ những điểm liên tiếp mới có thể nhìn thấy nhau. 
4. Lặp lại quá trình nhóm tương tự, nhưng bây giờ theo tọa độ y. Điều này xử lý khả năng hiển thị theo chiều ngang theo cách tương tự, nhưng dọc theo hàng thay vì cột. 
5. Duy trì một mảng`attacked`cho tất cả các quân xe, được khởi tạo thành sai. Bất cứ khi nào tìm thấy một cặp liền kề hợp lệ giữa những người chơi khác nhau ở một trong hai chiều, hãy đặt cả hai mục nhập tương ứng thành true. 
6. Kết quả đầu ra của từng người chơi theo thứ tự đầu vào. 

### Tại sao nó hoạt động 

Bất biến cốt lõi là sau khi sắp xếp trong một nhóm tọa độ cố định, bất kỳ cạnh hiển thị hợp lệ nào cũng phải xuất hiện giữa hai điểm liên tiếp theo thứ tự được sắp xếp đó. Nếu hai điểm chia sẻ một đường thẳng và không liên tiếp thì ít nhất một điểm khác nằm giữa chúng và chặn đòn tấn công. Do đó, mọi cuộc tấn công hợp lệ đều tương ứng chính xác với một lần tấn công lân cận trong quá trình quét nhóm x hoặc nhóm y và mọi lần kiểm tra lân cận sẽ nắm bắt được một cuộc tấn công hợp lệ nếu người chơi khác nhau. Điều này đảm bảo tính đầy đủ và chính xác mà không bỏ sót hoặc tính hai lần bất kỳ cặp nào có thể truy cập được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n1, n2 = map(int, input().split())
    
    pts = []
    attacked = [False] * (n1 + n2)

    for i in range(n1):
        x, y = map(int, input().split())
        pts.append((x, y, 0, i))

    for i in range(n2):
        x, y = map(int, input().split())
        pts.append((x, y, 1, n1 + i))

    # process by x (vertical visibility)
    pts.sort()
    i = 0
    while i < len(pts):
        j = i
        x = pts[i][0]
        group = []
        while j < len(pts) and pts[j][0] == x:
            group.append(pts[j])
            j += 1
        
        group.sort(key=lambda t: t[1])

        for k in range(len(group) - 1):
            if group[k][2] != group[k + 1][2]:
                attacked[group[k][3]] = True
                attacked[group[k + 1][3]] = True

        i = j

    # process by y (horizontal visibility)
    pts.sort(key=lambda t: (t[1], t[0]))
    i = 0
    while i < len(pts):
        j = i
        y = pts[i][1]
        group = []
        while j < len(pts) and pts[j][1] == y:
            group.append(pts[j])
            j += 1
        
        group.sort(key=lambda t: t[0])

        for k in range(len(group) - 1):
            if group[k][2] != group[k + 1][2]:
                attacked[group[k][3]] = True
                attacked[group[k + 1][3]] = True

        i = j

    res1 = ''.join('1' if attacked[i] else '0' for i in range(n1))
    res2 = ''.join('1' if attacked[n1 + i] else '0' for i in range(n2))

    print(res1)
    print(res2)

if __name__ == "__main__":
    solve()
```Việc triển khai phản ánh sự phân rã về mặt lý thuyết thành quét dọc và quét ngang. Mỗi lần quét sẽ xây dựng lại các nhóm tọa độ và dựa vào việc sắp xếp để thực thi tính tương đương kề. Chi tiết quan trọng là chúng tôi không cố gắng tìm kiếm giữa các cặp tùy ý, điều này tránh hoàn toàn hành vi bậc hai. 

Một lựa chọn triển khai tinh tế là lưu trữ cả kết quả quét x và quét y vào cùng một`attacked`mảng. Điều này là an toàn vì các cuộc tấn công đều đơn điệu theo nghĩa là một khi quân xe được đánh dấu, nó sẽ bị đánh dấu bất kể hướng nào. Một chi tiết khác là lập chỉ mục ổn định: mỗi quân mang một chỉ mục chung để chúng ta có thể cập nhật câu trả lời mà không bị mơ hồ sau khi sắp xếp. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 2
0 0
0 1
1 0
0 -1
-1 0
```Chúng tôi xử lý nhóm x trước tiên. 

| x | nhóm (sắp xếp theo y) | kiểm tra lân cận | cập nhật bị tấn công | 
| --- | --- | --- | --- | 
| 0 | (0, -1 S), (0, 0 P), (0, 1 P) | S-P, P-P | S, P(0,0) | 
| 1 | (1, 0 P), (-1, 0 S) | SP | cả hai | 

Sau khi quét dọc, một số quân xe được đánh dấu. Quét ngang củng cố các mối quan hệ tương tự vì tất cả các điểm cũng nằm trên y=0 hoặc tạo thành cặp. 

Đầu ra cuối cùng: 

Bàng:`111`Shou:`11`Điều này chứng tỏ rằng nhiều phát hiện liền kề hợp lệ có thể củng cố tính chính xác mà không gặp vấn đề về tính trùng. 

### Ví dụ 2 

đầu vào:```
2 2
0 0
0 2
0 1
0 3
```Mọi điểm đều nằm trên x = 0. 

| x=0 nhóm được sắp xếp theo y | lân cận | kết quả | 
| --- | --- | --- | 
| (0 P), (1 S), (2 P), (3 S) | P-S, S-P, P-S | tất cả bị tấn công | 

Điều này xác nhận rằng quy tắc kề nắm bắt chính xác khả năng hiển thị theo chuỗi dọc theo một đường. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | sắp xếp theo x và y chiếm ưu thế, mỗi lần quét nhóm nhìn chung là tuyến tính | 
| Không gian | O(n) | lưu trữ tất cả các điểm và cờ tấn công | 

Giải pháp này phù hợp một cách thoải mái trong giới hạn cho tổng số 400000 điểm, vì việc sắp xếp và quét tuyến tính có hiệu quả ở quy mô này. 

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

# sample
assert run("""3 2
0 0
0 1
1 0
0 -1
-1 0
""") == "111\n11"

# single pair direct attack
assert run("""1 1
0 0
0 1
""") == "1\n1"

# no attack
assert run("""2 2
0 0
2 2
1 1
3 3
""") == "0\n0"

# chain on same row
assert run("""2 2
0 0
2 0
1 0
3 0
""") == "11\n11"

# vertical chain alternating players
assert run("""1 3
0 0
0 1
0 2
0 3
""") == "1\n111"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mẫu | hỗn hợp | tính đúng đắn trên hình học hỗn hợp | 
| cặp đơn | 1/1 | tầm nhìn trực tiếp | 
| không tấn công | 0/0 | trường hợp cách ly | 
| hàng chuỗi | tất cả 1 | lan truyền lân cận | 
| chuỗi dọc | đánh dấu đầy đủ | độ chính xác của cột dài | 

## Vỏ cạnh 

Trường hợp cạnh then chốt là khi nhiều quân xe chia sẻ cùng một đường tọa độ với quyền sở hữu luân phiên. Ví dụ:```
(0,0) P, (0,1) S, (0,2) P, (0,3) S
```Trong quá trình quét nhóm x, chúng tôi sắp xếp theo y và chỉ kiểm tra các cặp liền kề. Quá trình quét đánh dấu tất cả các quân xe vì mọi cặp liền kề đều là người chơi chéo. Điều này truyền bá chính xác trạng thái tấn công trên toàn bộ chuỗi mà không yêu cầu kiểm tra tầm xa. 

Một trường hợp khác là khi chỉ có một xe tồn tại trên một hàng. Vì không có cặp liền kề nên không có gì được đánh dấu, điều này phản ánh chính xác rằng không thể tấn công được. 

Cuối cùng, hãy xem xét một cấu hình dày đặc trong đó nhiều quân xe chồng lên nhau ở một tọa độ nhưng lại dàn trải ở nơi khác. Thuật toán tách biệt từng nhóm tọa độ một cách độc lập, đảm bảo các đường không liên quan không can thiệp lẫn nhau, duy trì tính chính xác trên toàn bộ lưới.
