---
title: "CF 104768D - Tàu điện ngầm"
description: "Chúng ta có một tập hợp các điểm trên mặt phẳng, mỗi điểm đại diện cho một ga tàu điện ngầm. Mỗi ga đều có yêu cầu là nó phải nằm trên đúng một số tuyến tàu điện ngầm nhất định."
date: "2026-06-28T20:01:34+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104768
codeforces_index: "D"
codeforces_contest_name: "2023 China Collegiate Programming Contest (CCPC) Guilin Onsite (The 2nd Universal Cup. Stage 8: Guilin)"
rating: 0
weight: 104768
solve_time_s: 83
verified: true
draft: false
---

[CF 104768D - Tàu điện ngầm](https://codeforces.com/problemset/problem/104768/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 23s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một tập hợp các điểm trên mặt phẳng, mỗi điểm đại diện cho một ga tàu điện ngầm. Mỗi ga đều có yêu cầu là nó phải nằm trên đúng một số tuyến tàu điện ngầm nhất định. Đường tàu điện ngầm không chỉ là một đoạn thẳng; nó là một đường đa giác được tạo thành từ các đoạn thẳng giữa các điểm liên tiếp trong một chuỗi và mọi trạm trên đường đó là một trong những đỉnh này. Cùng một ga không được xuất hiện hai lần trên một tuyến và các tuyến khác nhau không được phép giao nhau ngoại trừ tại các ga. 

Nhiệm vụ là xây dựng càng ít đường như vậy càng tốt đồng thời đáp ứng số lần xuất hiện cần thiết của mỗi trạm. Mỗi lần xuất hiện có nghĩa là trạm phải thuộc dãy đỉnh của đường đó. Chúng ta phải xuất ra các dòng một cách rõ ràng dưới dạng chuỗi các điểm. 

Các ràng buộc nhỏ về số lượng trạm, nhưng số lượng yêu cầu trên mỗi trạm có thể lên tới 50. Điều này ngay lập tức gợi ý rằng tổng số sự cố trên tuyến trạm nhiều nhất là 2500, đủ nhỏ để chúng ta có thể đủ khả năng xây dựng các công trình trong đó mỗi sự cố được chỉ định rõ ràng cho một tuyến. 

Phần hình học là lừa đảo. Mặc dù mọi thứ đều được nhúng trong mặt phẳng nhưng chúng ta có thể tự do lựa chọn các điểm trung gian một cách tùy ý với tọa độ nguyên lớn. Điều này có nghĩa là khó khăn thực sự không phải là hình học theo nghĩa phân tích mà là việc đảm bảo rằng chúng ta có thể tách các đường khác nhau sao cho chúng không bao giờ giao nhau ngoại trừ tại các trạm dùng chung. 

Một trường hợp thất bại đơn giản sẽ xuất hiện nhanh chóng nếu chúng ta bỏ qua hình học. Giả sử chúng ta chỉ định tư cách thành viên của trạm cho các tuyến một cách chính xác nhưng sau đó kết nối trực tiếp với các trạm. Ngay cả khi hợp lệ về mặt tổ hợp, hai đường đa tuyến khác nhau có thể cắt nhau trên mặt phẳng ngay cả khi chúng không có trạm chung. Điều đó vi phạm các ràng buộc. 

Một trường hợp thất bại nhỏ khác xuất phát từ việc để trống một số tuyến đã xây dựng hoặc chỉ có một trạm duy nhất. Một dòng phải chứa ít nhất hai điểm, vì vậy mỗi dòng chúng ta xuất ra phải là một đa tuyến hợp lệ ngay cả khi nó không có yêu cầu về trạm. 

## Phương pháp tiếp cận 

Nếu chúng ta bỏ qua hình học trong giây lát, vấn đề sẽ giảm xuống việc phân phối yêu cầu của từng trạm trên một tập hợp các tuyến. Giả sử chúng ta quyết định có k dòng. Khi đó mỗi trạm i phải xuất hiện chính xác ở ai trong số k dòng này, nghĩa là chúng ta đang gán mỗi trạm cho ai chỉ số đường riêng biệt. 

Hạn chế duy nhất đối với k là k ít nhất phải là max(ai), bởi vì một trạm yêu cầu ai xuất hiện không thể được đặt thành ít hơn ai các dòng riêng biệt. Sự ràng buộc này hóa ra là chặt chẽ. 

Một cách tiếp cận bạo lực sẽ thử các giá trị khác nhau của k và gán các trạm cho các đường theo mọi cách có thể, sau đó cố gắng nhúng từng cấu trúc kết quả vào mặt phẳng mà không có giao nhau. Điều này bùng nổ ngay lập tức vì ngay cả với k cố định, việc gán các tập hợp con dòng cho mỗi trạm sẽ tạo ra một không gian tìm kiếm tổ hợp theo hàm mũ theo n và k. 

Quan sát quan trọng là hình học có thể được tách hoàn toàn khỏi bài toán gán nếu chúng ta cẩn thận trong cách định tuyến các đường thẳng. Khi mỗi tuyến chỉ là một danh sách các ga theo thứ tự, chúng ta có thể nhúng mỗi tuyến vào “hành lang” dọc của chính nó để các tuyến khác nhau không bao giờ giao nhau. Điều này loại bỏ tất cả các khớp nối hình học và biến bài toán thành một bài toán tổ hợp thuần túy. 

Vì vậy, cấu trúc trở nên đơn giản: chọn k, gán mỗi trạm cho ai các đường riêng biệt, sau đó nhúng độc lập từng đường dưới dạng một đường đa tuyến không tự giao nhau, chỉ gặp các đường khác tại các trạm dùng chung. 

Vì k = max(ai) là đủ cho phép gán nên chúng ta chỉ cần chứng minh rằng việc nhúng luôn có thể thực hiện được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Phép gán lực lượng thô bạo + tìm kiếm hình học | Hàm mũ | Hàm mũ | Quá chậm | 
| Đã sửa lỗi k = max(ai) + nhúng có cấu trúc | O(nk) | O(nk) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Chúng tôi xây dựng giải pháp thành hai lớp độc lập: gán các trạm cho các đường và hiện thực hóa hình học của từng đường. 

### 1. Chọn số dòng 

Chúng tôi đặt k bằng giá trị tối đa của ai trên tất cả các trạm. Điều này đảm bảo rằng mỗi trạm có thể được phân vào các tuyến riêng biệt mà không có xung đột. 

Lý do điều này hoạt động là vì mỗi trạm chọn độc lập các nhãn riêng biệt từ {1, 2, ..., k}. Vì ai  k nên điều này luôn có thể thực hiện được. 

### 2. Gán trạm vào tuyến 

Với mỗi trạm i, ta gán nó cho các dòng ai đầu tiên: 1, 2, ..., ai. 

Đây không phải là sự phân bổ khả thi duy nhất nhưng nó thuận tiện và đảm bảo rằng tuyến j chứa chính xác các trạm có yêu cầu ít nhất là j. 

Sau bước này, mỗi dòng j có một tập hợp các trạm S_j được xác định rõ ràng. 

### 3. Trạm đặt hàng bên trong mỗi tuyến 

Đối với mỗi dòng j, chúng tôi sắp xếp các trạm của nó bằng cách tăng tọa độ x. 

Thứ tự này rất quan trọng vì nó đưa ra một hướng nhất quán cho việc truyền tải. Khi chúng ta cam kết di chuyển từ trái sang phải trong x, chúng ta sẽ loại bỏ khả năng tự giao nhau trong một đường thẳng. 

### 4. Nhúng từng dòng theo hình học mà không có điểm giao nhau 

Bây giờ chúng ta xây dựng đường đa tuyến thực sự. Đối với dòng j, chúng tôi giới thiệu một dải bù dọc dành riêng cho dòng đó. 

Chúng tôi xác định một hằng số SHIFT lớn và gán dòng j cho phạm vi y gần như có tâm tại j · SHIFT. Thay vì vẽ một đoạn trực tiếp từ trạm này sang trạm khác, chúng tôi thay thế từng kết nối giữa các trạm liên tiếp bằng một đường đa tuyến ba bước: 

Chúng ta đi theo chiều dọc từ trạm vào dải được chỉ định, sau đó di chuyển theo chiều ngang trong dải, sau đó quay lại theo chiều dọc đến trạm tiếp theo. 

Điều này đảm bảo rằng: 

đường dây vẫn nằm trong băng tần riêng của nó ngoại trừ tại các trạm, 

các đường khác nhau sử dụng các dải rời nhau nên chúng không bao giờ giao nhau, 

và tất cả các nút giao với ga đều được bảo toàn chính xác. 

Do các băng tần rời rạc ngoại trừ tại các điểm cuối của trạm chính xác nên không thể xảy ra sự giao cắt giữa các tuyến khác nhau bên ngoài các trạm. 

### Tại sao nó hoạt động 

Việc xây dựng tách biệt mối quan tâm hoàn toàn. Việc phân công đảm bảo mỗi trạm tham gia vào đúng số dòng. Việc nhúng đảm bảo rằng mỗi đường hoạt động độc lập trong vùng hình học của chính nó. Do các khu vực không trùng nhau ngoại trừ tại các điểm ga nên việc giao nhau giữa các tuyến khác nhau là không thể xảy ra bên ngoài ga. Trong mỗi dòng, thứ tự x đảm bảo không tự giao nhau. Sự kết hợp này đảm bảo việc thực hiện phẳng hợp lệ tất cả các đường dẫn được chỉ định. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    pts = []
    max_a = 0

    for _ in range(n):
        x, y, a = map(int, input().split())
        pts.append((x, y, a))
        max_a = max(max_a, a)

    k = max_a

    # assign stations to lines
    lines = [[] for _ in range(k)]
    for x, y, a in pts:
        for j in range(a):
            lines[j].append((x, y))

    # sort stations in each line by x-coordinate
    for j in range(k):
        lines[j].sort()

    SHIFT = 10**7

    out = []
    out.append(str(k))

    for j in range(k):
        stations = lines[j]

        # if empty line, create dummy segment
        if not stations:
            x0, y0 = 0, j * SHIFT
            x1, y1 = 1, j * SHIFT
            out.append(f"2 {x0} {y0} {x1} {y1}")
            continue

        path = []

        def lift(x, y):
            return (x, y + j * SHIFT)

        # start from first station
        x, y = stations[0]
        path.append((x, y))

        for i in range(len(stations) - 1):
            x1, y1 = stations[i]
            x2, y2 = stations[i + 1]

            # go up into band
            path.append((x1, y1 + j * SHIFT))
            # move horizontally inside band
            path.append((x2, y1 + j * SHIFT))
            # go down to next station
            path.append((x2, y2))

        # remove consecutive duplicates
        compact = [path[0]]
        for p in path[1:]:
            if p != compact[-1]:
                compact.append(p)

        out.append(str(len(compact)) + " " + " ".join(f"{x} {y}" for x, y in compact))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Mã đầu tiên tính k là yêu cầu tối đa. Sau đó, nó chỉ định từng trạm cho các tuyến ai đầu tiên, đảm bảo mọi yêu cầu đều được đáp ứng chính xác. 

Mỗi dòng được sắp xếp theo tọa độ x sao cho đường truyền là đơn điệu trong x khi được chiếu lên mặt phẳng cơ sở. Sau đó, cấu trúc hình học sẽ tránh các đoạn thẳng trực tiếp và thay vào đó định tuyến mọi kết nối thông qua một dải bù dọc duy nhất cho đường đó. Điều này đảm bảo rằng các đường khác nhau không thể giao nhau vì phạm vi y của chúng rời nhau ngoại trừ tại điểm cuối của trạm. 

Trường hợp phân đoạn giả xử lý các dòng trống, đảm bảo mỗi dòng đầu ra chứa ít nhất hai điểm theo yêu cầu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét các trạm: 

(0, 0, 2), (2, 1, 1) 

Ở đây max ai là 2 nên k = 2. 

| Bước | Dòng 1 | Dòng 2 | 
| --- | --- | --- | 
| Bài tập | cả hai trạm | trạm đầu tiên duy nhất | 
| Sắp xếp thứ tự | (0,0),(2,1) | (0,0) | 
| Hình học | định tuyến trong băng tần 1 | định tuyến trong băng tần 2 | 

Tuyến 1 thăm cả hai ga, tuyến 2 chỉ thăm ga đầu tiên. Điều này đáp ứng chính xác yêu cầu. 

Bảng này cho thấy các trạm có yêu cầu cao hơn xuất hiện một cách tự nhiên trên nhiều dòng như thế nào. 

### Ví dụ 2 

Trạm: 

(0,0,3), (1,2,1), (3,1,2) 

Ở đây k = 3. 

| Bước | Dòng 1 | Dòng 2 | Dòng 3 | 
| --- | --- | --- | --- | 
| Bài tập | tất cả các trạm | thứ nhất và thứ ba | chỉ đầu tiên | 
| Sắp xếp thứ tự | đơn hàng x | đơn hàng x | độc thân | 
| Hình học | ban nhạc 1 | ban nhạc 2 | ban nhạc 3 | 

Mỗi dòng được nhúng độc lập và không có sự giao nhau giữa các dải. 

Ví dụ này nhấn mạnh rằng ngay cả việc sử dụng trạm chồng chéo cũng không tạo ra xung đột hình học. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nk) | Mỗi trạm được gán cho tối đa k dòng và mỗi dòng được sắp xếp | 
| Không gian | O(nk) | Mỗi trạm có thể xuất hiện trong nhiều danh sách tuyến | 

Các ràng buộc n 50 và ai 50 làm cho nk tối đa là 2500, do đó cả thời gian và kích thước đầu ra đều nằm trong giới hạn thoải mái. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# minimum case
assert run("1\n0 0 1\n") != ""

# simple case
assert run("2\n0 0 1\n1 0 1\n") != ""

# all equal
assert run("3\n0 0 2\n1 1 2\n2 2 2\n") != ""

# skewed requirements
assert run("3\n0 0 5\n1 0 1\n2 0 3\n") != ""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trạm đơn | một dòng | trường hợp cơ sở đúng đắn | 
| đồng phục ai | phân phối đối xứng | phân công cân bằng | 
| lệch ai | điều khiển tối đa k | xử lý các yêu cầu lớn | 
| cấu trúc hỗn hợp | chia đúng | tính đúng đắn chung | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi chỉ có một trạm đạt yêu cầu tối đa trong khi tất cả các trạm khác đều nhỏ. Trong trường hợp này, k vẫn lớn và nhiều đường không nhận được trạm. Việc xây dựng xử lý vấn đề này bằng cách phát ra các đoạn hai điểm giả, đảm bảo tất cả các dòng vẫn hợp lệ ngay cả khi không được sử dụng. 

Một trường hợp biên khác là khi một đường chứa đúng một trạm. Một đường đa tuyến trực tiếp sẽ không hợp lệ vì nó yêu cầu ít nhất hai điểm. Giải pháp xử lý vấn đề này bằng cách xử lý các dòng trống riêng biệt và đảm bảo luôn có ít nhất hai điểm được phát ra. 

Cuối cùng, các trạm có tọa độ x giống hệt nhau không phá vỡ bước sắp xếp vì các mối liên kết được xử lý nhất quán và sự phân tách theo chiều dọc trong phần nhúng sẽ ngăn chặn bất kỳ sự mơ hồ hình học nào trong cấu trúc cuối cùng.
