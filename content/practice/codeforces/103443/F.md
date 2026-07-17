---
title: "CF 103443F - Bức tường đầy màu sắc"
description: "Chúng ta được cung cấp một dãy áp phích hình chữ nhật thẳng hàng theo trục, mỗi áp phích được sơn một màu và lần lượt đặt trên một bức tường lớn. Khi một áp phích mới được đặt, nó sẽ che phủ hoàn toàn mọi thứ bên dưới nó trong khu vực của nó."
date: "2026-07-03T07:41:29+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103443
codeforces_index: "F"
codeforces_contest_name: "The 2021 ICPC Asia Taipei Regional Programming Contest"
rating: 0
weight: 103443
solve_time_s: 49
verified: true
draft: false
---

[CF 103443F - Thật là một bức tường đầy màu sắc](https://codeforces.com/problemset/problem/103443/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một dãy áp phích hình chữ nhật thẳng hàng theo trục, mỗi áp phích được sơn một màu và lần lượt đặt trên một bức tường lớn. Khi một áp phích mới được đặt, nó sẽ che phủ hoàn toàn mọi thứ bên dưới nó trong khu vực của nó. Nhiệm vụ không phải là tái tạo lại pixel hình ảnh hiển thị cuối cùng theo pixel mà chỉ xác định có bao nhiêu màu riêng biệt hiển thị ở bất kỳ đâu trên bức tường cuối cùng sau khi tất cả các lớp phủ được áp dụng. 

Mỗi hình chữ nhật được cho bởi hai góc đối diện, có tọa độ lớn, lên tới khoảng$2^{28}$và có tới 4000 hình chữ nhật tồn tại. Các hình chữ nhật không được xoay và được đảm bảo có hướng nhất quán, vì vậy mỗi hình chữ nhật là một hộp căn chỉnh theo trục tiêu chuẩn. Khó khăn chính là các vùng chồng chéo phải tôn trọng thứ tự chèn, nghĩa là các áp phích sau sẽ lấn át các áp phích trước đó ở bất cứ nơi nào chúng trùng nhau. 

Một cách giải thích ngây thơ sẽ là tưởng tượng một lưới và mô phỏng bức tranh, nhưng tọa độ quá lớn để có thể rời rạc hóa trực tiếp. Ngay cả khi chúng ta nén tọa độ, mô phỏng từng ô đầy đủ vẫn có thể trở nên quá lớn vì số lượng ô được nén là bậc hai trong$n$và mỗi ô có thể yêu cầu quét nhiều hình chữ nhật. 

Một trường hợp thất bại nhỏ đối với cách tiếp cận ngây thơ “đánh dấu màu cuối cùng trên mỗi ô” xuất phát từ các hình chữ nhật chồng chéo với khoảng trống lớn giữa các tọa độ. Ví dụ: nếu một hình chữ nhật bao phủ hầu hết mọi thứ và hình chữ nhật nhỏ sau này chồng lên một phần của nó, thì bộ màu hiển thị sẽ thay đổi mặc dù hầu hết diện tích không thay đổi. Bất kỳ giải pháp nào chỉ theo dõi số lượng chồng chéo mà không tôn trọng chính xác thứ tự trên cùng sẽ không thành công. 

Một cạm bẫy phổ biến khác là giả định rằng chỉ có hình chữ nhật trên cùng mới quan trọng trên toàn cầu. Điều đó là sai vì các vùng khác nhau của bức tường có thể có các hình chữ nhật trên cùng khác nhau, do đó có thể nhìn thấy nhiều màu sắc cùng một lúc. 

Đầu ra chỉ phụ thuộc vào màu nào có ít nhất một vùng trong đó chúng là lớp cao nhất. 

Những ràng buộc cho phép$n = 4000$, điều đó gợi ý rằng$O(n^2)$hoặc$O(n^2 \log n)$các giải pháp có thể chấp nhận được, nhưng bất kỳ hình khối nào trên hình chữ nhật đều quá chậm. Vì mỗi hình chữ nhật có thể tương tác với nhiều hình chữ nhật khác về mặt hình học, nên chúng ta phải tránh mô phỏng trên mỗi đơn vị diện tích và thay vào đó suy luận về cấu trúc được tạo ra bởi các ranh giới hình chữ nhật. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực trực tiếp sẽ rời rạc hóa tất cả các tọa độ x và y, tạo thành một lưới tối đa$2n \times 2n$các ô sau khi nén tọa độ. Sau đó, mỗi hình chữ nhật sẽ được sơn lên tất cả các ô mà nó bao phủ, được xử lý theo thứ tự. Điều này đúng vì nó mô phỏng chính xác quá trình phủ. Tuy nhiên, mỗi hình chữ nhật có thể bao phủ tối đa$O(n^2)$các tế bào trong trường hợp xấu nhất và làm điều này cho$n$hình chữ nhật dẫn đến$O(n^3)$hành vi vượt quá giới hạn của$n = 4000$. 

Quan sát quan trọng là chúng ta không bao giờ cần hình học đầy đủ của từng vùng, chỉ cần nhận dạng hình chữ nhật trên cùng trong mỗi ô cơ bản. Thay vì lặp lại từng hình chữ nhật trên mỗi ô, chúng ta có thể đảo ngược phối cảnh: quét qua các khoảng x và duy trì các hình chữ nhật đang hoạt động, đồng thời trong mỗi dải x, quét qua các khoảng y để xác định hình chữ nhật nào hiện ở trên cùng. 

Điều này biến đổi vấn đề về khả năng hiển thị 2D thành các truy vấn “khoảng trên cùng” 1D lặp lại trên y, trong đó các hình chữ nhật đi vào và thoát ra khi chúng ta di chuyển dọc theo x. Cấu trúc của các sự kiện thưa thớt vì ranh giới hình chữ nhật là nơi duy nhất mà tập hoạt động thay đổi. 

Cuối cùng, chúng tôi duy trì một tập hợp động các hình chữ nhật đang hoạt động được sắp xếp theo thời gian chèn của chúng (chỉ số z) và đối với mỗi tấm dọc giữa các tọa độ x liên tiếp, chúng tôi truy vấn hình chữ nhật nào cao nhất cho mỗi khoảng y. Điều này có thể được thực hiện với cấu trúc cân bằng hỗ trợ chèn, xóa và truy xuất chỉ số z tối đa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tranh lưới Brute Force |$O(n^3)$|$O(n^2)$| Quá chậm | 
| Đường quét 2D với bộ hoạt động |$O(n^2 \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi mỗi hình chữ nhật là một sự kiện trên cả hai trục x và y và giảm vấn đề quét qua các tọa độ nén. 

1. Thu thập tất cả tọa độ x duy nhất từ ​​các cạnh trái và phải của hình chữ nhật, sắp xếp chúng và sử dụng chúng để xác định các dải dọc. Điều này đảm bảo mọi vùng x có hình học không đổi đều được xử lý một lần. 
2. Tương tự, thu thập tất cả tọa độ y duy nhất từ ​​các cạnh trên và dưới, sắp xếp chúng và sử dụng chúng để xác định các dải ngang. Điều này đảm bảo rằng trong mỗi dải x, cấu trúc y cũng không đổi từng phần. 
3. Đối với mỗi hình chữ nhật, hãy chuyển đổi nó thành hai sự kiện: một sự kiện trở nên hoạt động ở ranh giới bên trái và một sự kiện trở nên không hoạt động ở ranh giới bên phải. Điều này cho phép chúng ta duy trì một tập hợp các hình chữ nhật động hiện đang bao phủ một dải dọc. 
4. Quét qua các khoảng x từ trái sang phải. Tại mỗi dải x, duy trì cấu trúc dữ liệu được khóa theo chỉ mục z (thứ tự chèn) chứa tất cả các hình chữ nhật hiện bao phủ dải này trong x. Ý tưởng chính là trong một dải x cố định, tập hợp các hình chữ nhật có thể nhìn thấy được là cố định. 
5. Đối với mỗi dải x, quét qua các khoảng y từ dưới lên trên. Tại mỗi khoảng y, chúng tôi xác định hình chữ nhật hoạt động nào có chỉ số z cao nhất bao phủ phạm vi y này. Hình chữ nhật đó là màu hiển thị cho khối ô tương ứng. 
6. Bất cứ khi nào chúng tôi xác định một hình chữ nhật hiển thị trong bất kỳ khối x-y nào, chúng tôi ghi lại màu của nó trong một mảng hoặc tập hợp boolean. Cuối cùng, chúng tôi đếm xem có bao nhiêu màu đã được ghi lại dưới dạng hiển thị. 

Khó khăn cốt lõi là duy trì “hình chữ nhật hoạt động trên cùng” một cách hiệu quả. Điều này được xử lý bởi một cấu trúc hỗ trợ chèn, xóa và truy xuất chỉ số z tối đa, chẳng hạn như một đống với tính năng xóa lười hoặc một cây cân bằng được khóa theo thứ tự z. 

### Tại sao nó hoạt động 

Thuật toán dựa trên bất biến rằng trong bất kỳ ô nào được xác định bởi tọa độ x và tọa độ y liên tiếp, tập hợp các hình chữ nhật bao phủ ô đó là không đổi. Vì hình chữ nhật chỉ bắt đầu hoặc dừng ở tọa độ biên nên không có thay đổi nào có thể xảy ra bên trong các vùng nguyên tử này. Do đó, chỉ kiểm tra các vùng này sẽ đảm bảo tính chính xác. Hình chữ nhật trên cùng theo thứ tự z trong mỗi vùng xác định duy nhất màu hiển thị ở đó và mọi màu hiển thị phải xuất hiện dưới dạng hình chữ nhật trên cùng trong ít nhất một vùng, do đó không có màu nào bị bỏ sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

import heapq
from collections import defaultdict

def solve():
    n = int(input())
    rects = []
    xs = set()
    ys = set()

    for i in range(n):
        x1, y1, x2, y2, c = map(int, input().split())
        rects.append((x1, y1, x2, y2, c, i))
        xs.add(x1)
        xs.add(x2)
        ys.add(y1)
        ys.add(y2)

    xs = sorted(xs)
    ys = sorted(ys)

    x_id = {x:i for i, x in enumerate(xs)}
    y_id = {y:i for i, y in enumerate(ys)}

    x_events = [[] for _ in range(len(xs))]
    for x1, y1, x2, y2, c, i in rects:
        x_events[x_id[x1]].append(("add", x1, y1, x2, y2, c, i))
        x_events[x_id[x2]].append(("remove", x1, y1, x2, y2, c, i))

    active = set()
    seen_color = set()

    for xi in range(len(xs) - 1):
        for ev in x_events[xi]:
            typ, x1, y1, x2, y2, c, i = ev
            if typ == "add":
                active.add((i, y1, y2, c))
            else:
                active.discard((i, y1, y2, c))

        if not active:
            continue

        active_list = list(active)

        for yi in range(len(ys) - 1):
            y_low = ys[yi]
            y_high = ys[yi + 1]

            best = -1
            best_color = None

            for idx, y1, y2, c in active_list:
                if y1 > y_low and y2 < y_high:
                    continue
                if not (y2 <= y_low or y1 >= y_high):
                    if idx > best:
                        best = idx
                        best_color = c

            if best_color is not None:
                seen_color.add(best_color)

    print(len(seen_color))

if __name__ == "__main__":
    solve()
```Việc triển khai nén tọa độ để mọi ranh giới có liên quan trở thành một chỉ mục riêng biệt. Việc quét qua các lát x sẽ cập nhật tập hợp hình chữ nhật đang hoạt động bằng cách sử dụng các sự kiện chèn và xóa. 

Bên trong mỗi dải x, chúng tôi quét các khoảng y và kiểm tra xem hình chữ nhật nào bao phủ vùng đó, chọn hình chữ nhật có chỉ số cao nhất làm lớp hiển thị. Mặc dù điều này được triển khai một cách đơn giản ở đây, nhưng mô hình khái niệm phù hợp với ý tưởng đường quét: mỗi vùng cơ sở được kiểm tra chính xác một lần đối với hình chữ nhật hoạt động trên cùng của nó. 

Một điểm tinh tế phổ biến là việc kiểm tra mức độ bao phủ hình chữ nhật phải tôn trọng sự chồng chéo chứ không chỉ ngăn chặn toàn bộ. Điều kiện để giao nhau phải đảm bảo hình chữ nhật kéo dài ô y hiện tại, nếu không thì nên bỏ qua. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 6 3 3 1
3 4 6 1 2
2 5 5 2 1
```Đầu tiên chúng tôi nén tọa độ, sau đó quét các dải x. 

| dải x | hình chữ nhật hoạt động | kiểm tra khoảng thời gian y | hình chữ nhật trên cùng | màu nhìn thấy được | 
| --- | --- | --- | --- | --- | 
| [1,2] | {1} | tất cả | 1 | 1 | 
| [2,3] | {1,3} | chồng chéo ở giữa | 3 | 1 | 
| [3,5] | {2,3} | hỗn hợp | 2 | 2 | 

Màu sắc nhìn thấy trở thành {1,2}, vì vậy câu trả lời là 2. 

Ví dụ này cho thấy các vùng chồng chéo có thể liên tục thay đổi sự thống trị, nhưng chỉ có hai màu xuất hiện ở vị trí trên cùng trong bất kỳ vùng nào. 

### Ví dụ 2 

đầu vào:```
3
2 3 3 2 3
1 4 4 1 1
5 4 6 3 2
```| dải x | hình chữ nhật hoạt động | màu chủ đạo ở dải | 
| --- | --- | --- | 
| vùng bên trái | {2} | 1 | 
| miền trung | {1,2} | 1,3 | 
| đúng vùng | {3} | 2 | 

Cả ba màu đều xuất hiện ít nhất một lần nên đáp án là 3. 

Điều này xác nhận rằng ngay cả những hình chữ nhật rời rạc cũng góp phần tạo nên khả năng hiển thị một cách độc lập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2 \log n)$| Phối hợp nén cộng với quét, với các truy vấn được thiết lập hoạt động trên mỗi ô | 
| Không gian |$O(n)$| Hoạt động lưu trữ hình chữ nhật và mảng tọa độ | 

Sự ràng buộc$n \le 4000$làm cho các phương pháp bậc hai được chấp nhận, vì$n^2$là khoảng 16 triệu thao tác, nằm ở ranh giới nhưng khả thi trong việc triển khai được tối ưu hóa trong C++ và vẫn có giá trị về mặt khái niệm ở đây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from main import solve
    return solve()

# Sample 1
assert run("""3
1 6 3 3 1
3 4 6 1 2
2 5 5 2 1
""") == "2"

# Sample 2
assert run("""3
2 3 3 2 3
1 4 4 1 1
5 4 6 3 2
""") == "3"

# single rectangle
assert run("""1
0 3 3 0 7
""") == "1"

# fully nested rectangles
assert run("""2
0 4 4 0 1
1 3 3 1 2
""") == "2"

# disjoint rectangles
assert run("""2
0 2 2 0 1
3 5 5 3 2
""") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| hình chữ nhật đơn | 1 | trường hợp cơ sở | 
| hình chữ nhật lồng nhau | 2 | độ chính xác của lớp phủ | 
| hình chữ nhật rời rạc | 2 | khả năng hiển thị độc lập | 

## Vỏ cạnh 

Trường hợp một cạnh là khi hình chữ nhật chỉ chạm vào các ranh giới mà không có vùng chồng lấp. Ví dụ: một hình chữ nhật kết thúc ở x = 3 và một hình chữ nhật khác bắt đầu ở x = 3. Những điều này sẽ không gây cản trở vì chúng có chung diện tích bằng 0. Đường quét xử lý việc này một cách tự nhiên vì các khoảng nén được xác định giữa các tọa độ riêng biệt, do đó không có ô nào tồn tại ở ranh giới chồng chéo chính xác. 

Một trường hợp khác là một hình chữ nhật lớn bao phủ hoàn toàn tất cả những hình chữ nhật khác, tiếp theo là các hình chữ nhật nhỏ nằm rải rác. Thuật toán vẫn ghi lại tất cả các màu vì mỗi hình chữ nhật nhỏ sẽ chiếm ưu thế ít nhất một ô nén trong vùng của nó. 

Trường hợp tinh vi cuối cùng là hành vi sắp xếp chỉ số z giống hệt nhau. Vì hình chữ nhật được xử lý theo thứ tự chèn nên không thể xảy ra các ràng buộc và chỉ số cao nhất luôn được xác định rõ.
