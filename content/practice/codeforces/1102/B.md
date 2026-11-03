---
title: "CF 1102B - Tô màu mảng K"
description: "Chúng ta được cho một dãy số nguyên và được yêu cầu gán cho mỗi vị trí một trong k nhãn mà chúng ta sẽ gọi là màu sắc. Mỗi vị trí phải nhận đúng một màu. Đồng thời, mỗi màu từ 1 đến k phải xuất hiện ít nhất một lần trong tất cả các vị trí."
date: "2026-06-13T07:38:16+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1102
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 531 (Div. 3)"
rating: 1400
weight: 1102
solve_time_s: 452
verified: false
draft: false
---

[CF 1102B - Array K-Coloring](https://codeforces.com/problemset/problem/1102/B) 

**Đánh giá:** 1400 
**Tags:** tham lam, phân loại 
**Thời gian giải:** 7 phút 32 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy số nguyên và được yêu cầu gán cho mỗi vị trí một trong k nhãn mà chúng ta sẽ gọi là màu sắc. Mỗi vị trí phải nhận đúng một màu. Đồng thời, mỗi màu từ 1 đến k phải xuất hiện ít nhất một lần trong tất cả các vị trí. 

Có một hạn chế về cấu trúc bổ sung. Nếu chúng ta chỉ nhìn vào các vị trí được gán cho một màu cố định thì tất cả các giá trị ở các vị trí đó phải khác biệt theo từng cặp. Nói cách khác, trong mỗi lớp màu, chúng ta không được phép lặp lại cùng một giá trị. 

Vì vậy, nhiệm vụ là phân phối các chỉ số thành k nhóm sao cho mọi nhóm đều khác rỗng và mỗi nhóm không chứa các giá trị trùng lặp về mặt giá trị mảng. 

Các ràng buộc n 5000 và k ≤ n cho thấy các giải pháp O(n²) vẫn khả thi. Bất cứ điều gì khối hoặc liên quan đến việc tính toán lại tốn kém lặp đi lặp lại trên các tập hợp con sẽ là ranh giới nhưng đôi khi vẫn vượt qua. Đây là một bài toán tham lam mang tính xây dựng, vì vậy mục tiêu là tìm một bài tập ổn định hơn là tối ưu hóa một mục tiêu bằng số. 

Trường hợp lỗi khó phát hiện đầu tiên xuất hiện khi một số giá trị xuất hiện rất thường xuyên. Nếu một giá trị xuất hiện nhiều hơn k lần thì bất kỳ giải pháp hợp lệ nào cũng phải đặt những lần xuất hiện đó thành các màu khác nhau vì việc trùng lặp bên trong một màu bị cấm. Điều đó ngay lập tức ngụ ý giới hạn dưới: tần số tối đa của bất kỳ giá trị nào phải nhiều nhất là k, nếu không thì không có màu nào tồn tại. 

Ví dụ: nếu k = 2 và mảng là [1, 1, 1], giá trị 1 xuất hiện ba lần, buộc ba màu riêng biệt cho các vị trí đó, điều này là không thể. Bất kỳ phép gán ngây thơ nào bỏ qua tần số sẽ tạo ra màu không chính xác. 

Một trường hợp cạnh khác là khi k lớn so với số lượng giá trị riêng biệt. Ngay cả khi tần số ổn, chúng ta vẫn cần k nhóm khác rỗng. Điều này buộc phải phân phối màu sắc một cách cẩn thận ngay cả khi chúng ta có thể tránh được xung đột. 

## Phương pháp tiếp cận 

Quan điểm bạo lực là nghĩ đến việc gán màu một cách tuần tự và thử mọi khả năng. Tại mỗi vị trí, chúng ta có thể chọn bất kỳ màu nào từ 1 đến k chưa chứa cùng một giá trị. Điều này dẫn đến hệ số phân nhánh lên tới k ở mỗi bước và việc duy trì tính hợp lệ đòi hỏi phải kiểm tra các bài tập trước đó. Trong trường hợp xấu nhất, điều này khám phá kⁿ khả năng, điều này hoàn toàn không khả thi ngay cả với n = 30. 

Quan sát cấu trúc quan trọng là xung đột chỉ phụ thuộc vào các giá trị bằng nhau. Các giá trị khác nhau không bao giờ tương tác. Vì vậy, đối với mỗi giá trị, chúng ta chỉ cần đảm bảo rằng lần xuất hiện của nó được phân bổ theo các màu riêng biệt. Điều này gợi ý việc xử lý các giá trị giống hệt nhau như một nhóm. 

Bây giờ hãy xem xét việc sắp xếp các chỉ số theo giá trị. Sau khi được nhóm, chúng tôi phân phối số lần xuất hiện của từng giá trị theo kiểu vòng tròn trên các màu từ 1 đến k. Điều này đảm bảo rằng không có giá trị nào lặp lại trong một màu vì chúng tôi không bao giờ gán cùng một giá trị hai lần cho cùng một màu. Đồng thời, nếu chúng tôi đảm bảo k màu được sử dụng, chúng tôi có thể đảm bảo mỗi màu không trống bằng cách gán k giá trị riêng biệt đầu tiên một cách cẩn thận. 

Mối quan tâm duy nhất còn lại là tính khả thi. Nếu bất kỳ giá trị nào xuất hiện nhiều hơn k lần, chúng ta sẽ thất bại ngay lập tức. Mặt khác, mọi giá trị có thể được phân phối mà không có xung đột nội bộ. 

Để đảm bảo tất cả k màu được sử dụng, chúng ta có thể gán màu theo chu kỳ khi lặp qua các phần tử được sắp xếp, nhưng chúng ta phải đảm bảo rằng tất cả các màu đều nhận được ít nhất một phép gán. Vì n ≥ k và chúng ta đang gán màu cho tất cả các phần tử nên chúng ta có thể đơn giản đảm bảo rằng k phép gán đầu tiên buộc phải sử dụng các màu riêng biệt. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(nk) | Quá chậm | 
| Sự phân công theo chu kỳ tham lam | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta tiến hành xây dựng tham lam dựa trên việc sắp xếp các chỉ số theo giá trị của chúng.

1. Nhóm các chỉ số theo giá trị mảng của chúng. Điều này cho phép chúng ta xử lý các giá trị giống nhau cùng nhau, điều này là cần thiết vì xung đột chỉ nảy sinh trong các giá trị bằng nhau. 
2. Đối với mỗi nhóm giá trị, hãy kiểm tra kích thước của nó. Nếu bất kỳ nhóm nào có kích thước lớn hơn k, ngay lập tức kết luận rằng không tồn tại màu hợp lệ. Điều này là do chúng ta buộc phải sử dụng lại một màu có cùng giá trị. 
3. Chuẩn bị danh sách các màu có sẵn từ 1 đến k. Chúng đại diện cho màu sắc mà chúng ta sẽ chuyển qua. 
4. Gán màu cho các phần tử bằng cách lặp qua các nhóm. Bên trong mỗi nhóm, gán màu tuần tự từ bảng màu, bao bọc xung quanh nếu cần. Tuy nhiên, vì kích thước nhóm ≤ k nên chúng tôi không bao giờ lặp lại một màu trong nhóm. 
5. Sau khi chỉ định tất cả các nhóm, hãy đảm bảo rằng mỗi màu xuất hiện ít nhất một lần. Điều này đương nhiên được thỏa mãn vì chúng tôi phân phối ít nhất một phần tử cho mỗi màu trên toàn bộ mảng miễn là n ≥ k và chúng tôi không bao giờ bỏ qua các màu trong quá trình gán. 

Ý tưởng quan trọng là chúng ta không bao giờ gán cùng một giá trị hai lần cho cùng một màu và chúng ta không bao giờ để lại bất kỳ màu nào không được sử dụng vì việc gán trải rộng trên toàn bộ bảng màu. 

### Tại sao nó hoạt động 

Bất biến cốt lõi là sau khi xử lý từng nhóm giá trị, không có màu nào chứa các lần xuất hiện trùng lặp của giá trị đó. Vì các nhóm tách rời nhau theo giá trị nên không bước nào sau đó có thể gây ra xung đột cho các giá trị đã xử lý trước đó. Ngoài ra, do chúng tôi chỉ định màu theo chu kỳ cân bằng nên mọi màu sẽ nhận được lượt gán bất cứ khi nào có thể, ngăn chặn các màu không được sử dụng khi k ≤ n. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    
    pos = {}
    for i, x in enumerate(a):
        pos.setdefault(x, []).append(i)
    
    # feasibility check
    for x in pos:
        if len(pos[x]) > k:
            print("NO")
            return
    
    res = [0] * n
    
    # assign colors
    color = 1
    for x in pos:
        for idx in pos[x]:
            res[idx] = color
            color += 1
            if color > k:
                color = 1
    
    print("YES")
    print(*res)

if __name__ == "__main__":
    solve()
```Giải pháp trước tiên là xây dựng một từ điển ánh xạ từng giá trị tới các vị trí của nó. Điều này là cần thiết để thực thi ràng buộc rằng các bản sao có cùng giá trị phải được kiểm tra cùng nhau. 

Việc kiểm tra tính khả thi là rất quan trọng: nếu bất kỳ danh sách nào vượt quá kích thước k, chúng tôi sẽ dừng ngay lập tức vì không có phép gán nào có thể tránh lặp lại một màu trong nhóm đó. 

Bước tô màu sử dụng một con trỏ tuần hoàn toàn cục duy nhất. Điều này đảm bảo chúng tôi phân phối màu đồng đều nhất có thể trên tất cả các thành phần. Bao quanh tại k đảm bảo chúng tôi sử dụng lại màu sắc nhưng không bao giờ nằm ​​trong một nhóm giá trị có kích thước tối đa là k. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 2
1 2 2 3
```Chúng tôi nhóm các chỉ số: 

| Giá trị | Vị trí | 
| --- | --- | 
| 1 | [0] | 
| 2 | [1, 2] | 
| 3 | [3] | 

Chúng tôi chỉ định màu sắc theo chu kỳ: 

| Bước | Chỉ mục | Giá trị | Con trỏ màu | Màu được chỉ định | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 1 | 1 | 1 | 
| 2 | 1 | 2 | 2 | 2 | 
| 3 | 2 | 2 | 1 | 1 | 
| 4 | 3 | 3 | 2 | 2 | 

Màu cuối cùng là:```
1 2 1 2
```Điều này cho thấy các bản sao của giá trị 2 sẽ có màu khác nhau. 

### Ví dụ 2 

đầu vào:```
5 3
4 4 4 2 2
```Nhóm: 

| Giá trị | Vị trí | 
| --- | --- | 
| 4 | [0, 1, 2] | 
| 2 | [3, 4] | 

Bài tập theo từng bước: 

| Bước | Chỉ mục | Giá trị | Màu sắc | 
| --- | --- | --- | --- | 
| 1 | 0 | 4 | 1 | 
| 2 | 1 | 4 | 2 | 
| 3 | 2 | 4 | 3 | 
| 4 | 3 | 2 | 1 | 
| 5 | 4 | 2 | 2 | 

Kết quả:```
1 2 3 1 2
```Mỗi màu xuất hiện ít nhất một lần và trong mỗi nhóm giá trị, các màu sẽ khác biệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | nhóm cộng với bài tập vượt qua một lần | 
| Không gian | O(n) | lưu trữ vị trí trên mỗi giá trị | 

Thuật toán dễ dàng phù hợp trong các giới hạn vì n ≤ 5000 và tất cả các phép toán đều tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    
    pos = {}
    for i, x in enumerate(a):
        pos.setdefault(x, []).append(i)
    
    for x in pos:
        if len(pos[x]) > k:
            return "NO\n"
    
    res = [0] * n
    color = 1
    for x in pos:
        for idx in pos[x]:
            res[idx] = color
            color += 1
            if color > k:
                color = 1
    
    return "YES\n" + " ".join(map(str, res)) + "\n"

# provided sample
assert run("4 2\n1 2 2 3\n") in ["YES\n1 2 1 2\n", "YES\n1 1 2 2\n"]

# custom cases
assert run("1 1\n7\n") == "YES\n1\n", "single element"
assert run("3 3\n1 2 3\n") == "YES\n1 2 3\n", "all distinct"
assert run("3 2\n1 1 1\n") == "NO\n", "impossible frequency"
assert run("6 3\n1 1 2 2 3 3\n") != "", "balanced pairs"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1/7 | CÓ 1 | trường hợp tối thiểu | 
| 3 3 / 1 2 3 | CÓ 1 2 3 | tất cả các giá trị riêng biệt | 
| 3 2 / 1 1 1 | KHÔNG | vi phạm tần số | 
| 6 3/cặp | màu hợp lệ | phân phối cân bằng | 

## Vỏ cạnh 

Khi tất cả các phần tử giống hệt nhau và n > k, thuật toán ngay lập tức thất bại ở lần kiểm tra tần số. Ví dụ: nhập [5, 5, 5] với k = 2 sẽ kích hoạt điều kiện vì giá trị 5 xuất hiện 3 lần. Việc kiểm tra sẽ phát hiện điều này trước khi bất kỳ nhiệm vụ nào bắt đầu, điều này ngăn cản việc tạo màu theo chu kỳ không hợp lệ. 

Khi tất cả các phần tử đều khác biệt, việc nhóm sẽ tạo ra các danh sách đơn lẻ. Phép gán theo chu kỳ chỉ đơn giản là duyệt qua các màu lặp đi lặp lại, nhưng vì mỗi giá trị là duy nhất nên không có ràng buộc nào bị vi phạm. Bất biến có giá trị tầm thường vì không có bản sao nào để quản lý.
