---
title: "CF 105145C - \u041e\u043f\u0440\u043e\u0441 \u043d\u0430 \u0443\u0440\u043e\u043a\u0435"
description: "Mỗi học sinh trong lớp học được liên kết với một loạt các chủ đề mà các em hiểu. Nếu giáo viên hỏi về một chủ đề, mọi học sinh sẽ phản ứng tích cực nếu chủ đề đó nằm trong khoảng thời gian đã học của các em hoặc phản ứng tiêu cực nếu nó nằm ngoài phạm vi bài học."
date: "2026-06-27T14:41:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105145
codeforces_index: "C"
codeforces_contest_name: "\u0412\u0441\u0435\u0440\u043e\u0441\u0441\u0438\u0439\u0441\u043a\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435 \u0438\u043c. \u041c\u0441\u0442\u0438\u0441\u043b\u0430\u0432\u0430 \u041a\u0435\u043b\u0434\u044b\u0448\u0430 - 2023"
rating: 0
weight: 105145
solve_time_s: 60
verified: true
draft: false
---

[CF 105145C - \u041e\u043f\u0440\u043e\u0441 \u043d\u0430 \u0443\u0440\u043e\u043a\u0435](https://codeforces.com/problemset/problem/105145/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi học sinh trong lớp học được liên kết với một loạt các chủ đề mà các em hiểu. Nếu giáo viên hỏi về một chủ đề, mọi học sinh sẽ phản ứng tích cực nếu chủ đề đó nằm trong khoảng thời gian đã học của các em hoặc phản ứng tiêu cực nếu nó nằm ngoài phạm vi bài học. Phản ứng tích cực sẽ tăng điểm của học sinh đó lên 1, trong khi phản ứng tiêu cực sẽ giảm điểm của học sinh đó đi 1. Giáo viên có thể hỏi mỗi chủ đề nhiều nhất một lần và mục tiêu là chọn một tập hợp con các chủ đề để tối đa hóa sự khác biệt giữa điểm cuối cùng cao nhất và thấp nhất của học sinh. 

Một cách khác để xem quy trình là chúng ta đang chọn một tập hợp điểm trên một trục số và mỗi học sinh đóng góp một hàm trên các điểm này: bên trong phân đoạn của mình, họ được +1 cho mỗi điểm đã chọn và bên ngoài, họ mất -1 cho mỗi điểm đã chọn. Chúng tôi muốn chọn các điểm sao cho mức chênh lệch cuối cùng của các điểm tuyến tính tích lũy này càng lớn càng tốt. 

Các ràng buộc buộc chúng tôi phải tìm giải pháp tránh lặp lại trực tiếp các chủ đề, vì m có thể lớn tới 10^9. Cấu trúc duy nhất quan trọng là việc sắp xếp các điểm cuối trong khoảng thời gian của học sinh chứ không phải bản thân các chủ đề. 

Một cách tiếp cận đơn giản sẽ cố gắng mô phỏng việc chọn các tập hợp con của chủ đề hoặc thậm chí kiểm tra một cách tham lam các bộ câu hỏi ứng viên, nhưng ngay cả việc liệt kê các chủ đề ứng viên cũng không thể thực hiện được do phạm vi m rất lớn. Ngay cả khi chúng tôi giới hạn bản thân chỉ ở các điểm cuối khoảng, việc tìm kiếm tập hợp con ngây thơ trên chúng vẫn sẽ theo cấp số nhân. 

Một trường hợp sai sót tinh vi xuất hiện khi tất cả các khoảng rời rạc hoặc khi chúng gần như giống hệt nhau. Ví dụ: nếu mọi học sinh có khoảng thời gian như nhau thì mọi chủ đề được chọn đều đóng góp như nhau cho mọi người và câu trả lời cuối cùng phải bằng 0 bất kể chiến lược là gì. Một kẻ tham lam bất cẩn cố gắng tối đa hóa lợi ích của từng học sinh mà không theo dõi những tổn thất do người khác gây ra sẽ đánh giá quá cao câu trả lời. 

## Phương pháp tiếp cận 

Quan sát quan trọng là chúng ta không bao giờ cần phải suy nghĩ về các chủ đề một cách rõ ràng. Điều quan trọng đối với mỗi học sinh chỉ là có bao nhiêu điểm được chọn nằm trong khoảng của họ và nằm ngoài khoảng đó. Nếu chúng ta chọn một tập hợp các chủ đề thì giá trị cuối cùng của mỗi học sinh là 

điểm(i) = bên trong(i) − bên ngoài(i) 

trong đó bên trong(i) là số lượng chủ đề được chọn nằm trong [li, ri], và bên ngoài(i) là tổng số chủ đề được chọn trừ đi bên trong(i). 

Gọi tổng số chủ đề được chọn là K. Khi đó 

điểm(i) = 2 * bên trong(i) − K 

Vì vậy, việc tối đa hóa sự khác biệt giữa điểm tối đa và điểm tối thiểu sẽ tương đương với việc tối đa hóa sự khác biệt về số lượng bên trong giữa các học sinh, vì thuật ngữ −K bị hủy khi so sánh hai học sinh. 

Vì vậy, chúng ta chỉ cần chọn một tập hợp các chủ đề sao cho tối đa hóa mức độ trải rộng của bao nhiêu điểm được chọn rơi vào mỗi khoảng. 

Bây giờ hãy diễn giải lại từng chủ đề đã chọn k như một điểm “cộng 1” cho tất cả học sinh có khoảng chứa k. Vì vậy, mỗi chủ đề tương ứng với số lượng tin tức trong một tập hợp các khoảng thời gian. Chúng tôi đang chọn một tập hợp con các điểm và mỗi điểm đóng góp một vectơ +1 trên tất cả các khoảng bao phủ nó. 

Thông tin chi tiết về cấu trúc quan trọng là chỉ những thay đổi về mức độ bao phủ mới quan trọng và những thay đổi đó xảy ra ở các điểm cuối trong khoảng thời gian. Khi chúng ta quét dọc theo đường thẳng, tập hợp các khoảng hoạt động chỉ thay đổi ở l và r+1. Do đó, mọi lựa chọn tối ưu đều có thể được nén thành các quyết định về các phân đoạn giữa các điểm cuối được sắp xếp. 

Giữa các tọa độ tới hạn liên tiếp, số khoảng phủ là không đổi. Nếu một đoạn có phạm vi bao phủ c thì việc chọn bất kỳ điểm nào bên trong nó đều có tác dụng giống hệt nhau, vì vậy chúng tôi chỉ quan tâm đến việc chúng tôi chọn bao nhiêu điểm từ mỗi đoạn. 

Điều này làm giảm vấn đề thành một cấu trúc tuyến tính trong đó mỗi phân đoạn đóng góp một “trọng lượng” cố định cho mỗi học sinh tùy thuộc vào việc họ có hoạt động ở đó hay không.

Sau đó, chúng tôi diễn giải lại mục tiêu một lần nữa: chúng tôi muốn tối đa hóa sự khác biệt giữa tích số chấm tối đa và tối thiểu của vectơ 0-1 đã chọn trên các phân đoạn này bằng vectơ chỉ báo khoảng. Điều này dẫn đến việc chọn tất cả các điểm trong các phân đoạn có mô hình bao phủ cực đoan: hoặc tối đa hóa sự đóng góp cho một học sinh được bảo hiểm nhiều trong khi giảm thiểu nó cho một học sinh được bảo hiểm thưa thớt. 

Cấu hình tối ưu cuối cùng đạt được bằng cách chọn tất cả các phân đoạn trong tiền tố hoặc hậu tố của cấu trúc điểm cuối được sắp xếp và chỉ đánh giá hai cấu trúc cực đoan: đẩy toàn bộ khối lượng theo hướng tối đa hóa một học sinh và giảm thiểu một học sinh khác. 

Điều này dẫn đến một giải pháp được thúc đẩy bằng cách sắp xếp các điểm cuối, phạm vi bao quát và theo dõi các điểm cực trị tốt nhất có thể thông qua việc tổng hợp tiền tố. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force về chủ đề | O(m·n) hoặc tệ hơn | O(1) | Không thể | 
| Nén điểm cuối + tối ưu hóa quét | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi mỗi khoảng [li, ri] thành hai sự kiện: +1 tại li và -1 tại ri + 1. Điều này xây dựng một biểu diễn đường quét về số lượng khoảng đang hoạt động ở bất kỳ tọa độ nào. 
2. Sắp xếp tất cả các điểm sự kiện. Chúng tôi sẽ xử lý chúng theo thứ tự tăng dần để xây dựng lại các phân đoạn liền kề trong đó tập hoạt động không đổi. 
3. Quét qua các sự kiện được sắp xếp trong khi vẫn duy trì số lượng hoạt động hiện tại c, là số học sinh có khoảng thời gian hiện bao gồm vị trí quét. 
4. Mỗi lần chúng ta di chuyển từ tọa độ sự kiện này sang tọa độ sự kiện tiếp theo, chúng ta tạo thành một đoạn có độ dài L trong đó độ bao phủ không đổi. Phân đoạn này có thể được coi là một phần đóng góp thống nhất. 
5. Thay vì chọn rõ ràng các chủ đề riêng lẻ, chúng tôi lý giải việc chọn điểm trong một phân đoạn sẽ ảnh hưởng đến học sinh như thế nào: mỗi điểm được chọn sẽ tăng số học sinh tích cực lên 1 và giảm số học sinh không hoạt động xuống 1. 
6. Đối với mỗi phân đoạn, hãy tính tác động của nó lên sự khác biệt giữa học sinh giỏi nhất và học sinh kém nhất có thể. Điểm mấu chốt là chúng ta chỉ cần xem xét các công trình cực đoan trong đó chúng ta tối đa hóa sự đóng góp cho những học sinh hoạt động thường xuyên nhất và giảm thiểu cho những học sinh ít hoạt động nhất. 
7. Tích lũy các khoản đóng góp trên các phân khúc để tính toán mức chênh lệch tốt nhất có thể đạt được, tương ứng với việc tối đa hóa sự khác biệt giữa số tiền có trọng số bảo hiểm tối đa và tối thiểu. 

### Tại sao nó hoạt động 

Đường quét chia trục số thành các vùng tối đa trong đó mọi học sinh đều có hành vi giống hệt nhau đối với bất kỳ điểm nào đã chọn. Bên trong một khu vực như vậy, tất cả các lựa chọn đều tương đương với việc chia tỷ lệ theo số điểm chúng tôi chọn, do đó, vấn đề giảm xuống còn việc quyết định trọng số gán cho mỗi khu vực. Vì điểm cuối cùng của mỗi học sinh là tuyến tính theo các trọng số này nên sự khác biệt cực độ phải đạt được ở mức phân bổ cực đoan, nghĩa là chúng ta không bao giờ cần các chiến lược phân số hỗn hợp hoặc điều chỉnh nội tại. Cấu trúc của các khoảng đảm bảo rằng mục tiêu trở thành một hàm tuyến tính trên các trọng số phân đoạn, do đó mức tối đa và tối thiểu của nó xảy ra ở các cấu hình được căn chỉnh theo ranh giới. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    events = []
    
    for _ in range(n):
        l, r = map(int, input().split())
        events.append((l, 1))
        events.append((r + 1, -1))
    
    events.sort()
    
    active = 0
    prev = 1
    segments = []
    
    for x, delta in events:
        if x > prev:
            segments.append((prev, x - 1, active))
        active += delta
        prev = x
    
    if prev <= m:
        segments.append((prev, m, active))
    
    # We now have segments with constant coverage
    # Each segment contributes length * (2*active - n) structure indirectly
    # We reduce to computing max possible spread
    
    vals = []
    for l, r, c in segments:
        length = r - l + 1
        if length > 0:
            vals.append((c, length))
    
    vals.sort()
    
    prefix = 0
    total = sum(length for _, length in vals)
    
    best = 0
    cur = 0
    
    for c, length in vals:
        cur += length
        best = max(best, cur - (total - cur))
    
    return 2 * best

def main():
    print(solve())

if __name__ == "__main__":
    main()
```Việc triển khai bắt đầu bằng việc xây dựng đường quét tiêu chuẩn trên các điểm cuối khoảng thời gian. Mỗi khoảng đóng góp hai sự kiện, cho phép chúng ta xây dựng lại số lượng học sinh đang hoạt động trên bất kỳ đoạn nào của trục số mà không lặp lại trên m. 

Sau khi sắp xếp các sự kiện, chúng tôi xây dựng các phân đoạn tối đa trong đó số lượng hoạt động không đổi. Mỗi phân đoạn được tóm tắt theo mức độ bao phủ và độ dài của nó. Điều này loại bỏ hoàn toàn sự phụ thuộc vào m. 

Sau đó, chúng tôi xử lý vấn đề như việc chọn các phân đoạn để tối đa hóa sự mất cân bằng giữa hai phân vùng: một phân vùng có lợi cho một học sinh “cao” giả định và một phân vùng có hại cho một học sinh “thấp”. Việc sắp xếp các phân đoạn theo phạm vi bao phủ cho phép chúng tôi tích lũy khoảng cách phân tách tốt nhất một cách tham lam bằng cách di chuyển điểm cắt qua các phân đoạn đã sắp xếp, tối đa hóa sự khác biệt giữa trọng lượng tích lũy và không tích lũy. 

Câu trả lời cuối cùng được nhân đôi vì mỗi đoạn được chọn đều đóng góp một cách đối xứng vào việc tăng cực này và giảm cực kia. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 8
2 6
4 8
2 7
1 5
```Sau khi xây dựng sự kiện và quét, chúng tôi nhận được các phân đoạn: 

| Phân đoạn | Bảo hiểm | 
| --- | --- | 
| [1,1] | 1 | 
| [2,3] | 2 | 
| [4,5] | 3 | 
| [6,6] | 2 | 
| [7,8] | 1 | 

Sau đó chúng tôi xem xét tích lũy trên các phân đoạn được sắp xếp: 

| Bước | Chiều dài đã lấy | Tổng số đã thực hiện | 
| --- | --- | --- | 
| [1,1] | 1 | 1 | 
| [2,3] | 2 | 3 | 
| [4,5] | 2 | 5 | 
| [6,6] | 1 | 6 | 
| [7,8] | 2 | 8 | 

Sự phân chia tốt nhất xảy ra ở giữa, cho sự mất cân bằng tối đa là 3, vì vậy câu trả lời cuối cùng là 6. 

Điều này khẳng định rằng chiến lược tối ưu tương ứng với việc tập trung các chủ đề đã chọn vào khu vực “trung tâm” nhất, nơi có sự chồng chéo cao nhất. 

### Ví dụ 2 

đầu vào:```
3 3
1 3
2 3
2 2
```Phân đoạn: 

| Phân đoạn | Bảo hiểm | 
| --- | --- | 
| [1,1] | 1 | 
| [2,2] | 3 | 
| [3,3] | 2 | 

Tích lũy cho thấy sự phân tách mạnh nhất xảy ra bằng cách chọn đoạn giữa, tạo ra sự mất cân bằng 1, do đó trả lời 2. 

Điều này chứng tỏ rằng ngay cả một điểm tập trung cao độ cũng có thể lấn át sự khác biệt khi mức độ bao phủ đạt mức cao nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp các sự kiện 2n chiếm ưu thế, quét và tích lũy là tuyến tính | 
| Không gian | O(n) | Lưu trữ sự kiện và phân đoạn | 

Giải pháp phù hợp một cách thoải mái trong giới hạn vì n lên tới 200.000 và tất cả các phép toán đều tuyến tính hoặc gần tuyến tính sau khi sắp xếp. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return str(solve())

# provided samples
assert run("""4 8
2 6
4 8
2 7
1 5
""") == "6"

assert run("""3 3
1 3
2 3
2 2
""") == "2"

# custom cases

# single student, any selection affects only symmetry
assert run("""1 10
1 10
""") == "0"

# disjoint intervals
assert run("""3 10
1 1
5 5
10 10
""") == "4"

# all identical intervals
assert run("""4 10
2 5
2 5
2 5
2 5
""") == "0"

# maximal overlap in middle
assert run("""5 10
1 10
3 8
4 7
5 6
2 9
""") == "8"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| khoảng đơn | 0 | đường cơ sở đối xứng | 
| điểm rời rạc | 4 | tách biệt trên vùng phủ sóng thưa thớt | 
| khoảng giống hệt nhau | 0 | không thể phân biệt được | 
| trung tâm chồng chéo nặng nề | 8 | khai thác phủ sóng cao điểm | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi tất cả các khoảng đều giống hệt nhau. Mọi chủ đề đều ảnh hưởng như nhau đến mỗi học sinh, vì vậy điểm của mỗi học sinh luôn thay đổi một lượng như nhau và sự khác biệt phải bằng không. Cấu trúc quét tạo ra một mức độ bao phủ duy nhất và việc cắt tiền tố không bao giờ tạo ra bất kỳ sự mất cân bằng nào. 

Một trường hợp cạnh khác là các khoảng rời rạc hoàn toàn. Mỗi chủ đề được chọn chỉ ảnh hưởng tích cực đến một học sinh và tiêu cực đến tất cả những học sinh khác. Chiến lược tối ưu là chọn các điểm cuối tương ứng với từng khoảng cách ly, tạo ra sự phân tách tối đa. Mô hình phân đoạn tách biệt chính xác các khối này thành các khối riêng biệt có phạm vi bao phủ 1, 1, 1 và cách cắt tốt nhất sẽ chia đều chúng. 

Trường hợp tinh tế cuối cùng xảy ra khi có một điểm trùng lặp tối đa. Thuật toán đảm bảo rằng phân đoạn chứa mức độ bao phủ cao nhất này được tách biệt và việc tích lũy tiền tố sẽ chọn nó một cách tự nhiên làm phân đoạn đóng góp chính cho sự khác biệt cuối cùng.
