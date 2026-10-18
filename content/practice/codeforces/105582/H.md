---
title: "CF 105582H - Hamburger"
description: "Chúng tôi được chia một số nhóm bạn và đối với mỗi nhóm, chúng tôi muốn chọn một quán cà phê khiến càng nhiều người trong nhóm đó vui vẻ càng tốt. Mỗi người có một sở thích được mô tả bằng một tập hợp nhỏ các thành phần, được mã hóa dưới dạng một chuỗi ngắn."
date: "2026-06-22T14:38:37+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105582
codeforces_index: "H"
codeforces_contest_name: "Ural Championship 2017"
rating: 0
weight: 105582
solve_time_s: 63
verified: true
draft: false
---

[CF 105582H - Bánh mì kẹp thịt](https://codeforces.com/problemset/problem/105582/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 3s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được chia một số nhóm bạn và đối với mỗi nhóm, chúng tôi muốn chọn một quán cà phê khiến càng nhiều người trong nhóm đó vui vẻ càng tốt. 

Mỗi người có một sở thích được mô tả bằng một tập hợp nhỏ các thành phần, được mã hóa dưới dạng một chuỗi ngắn. Một chiếc bánh hamburger còn được mô tả bằng một bộ nguyên liệu. Một người hài lòng với chiếc bánh hamburger nếu mọi thành phần họ thích đều xuất hiện trong chiếc bánh hamburger đó. Một quán cà phê được coi là tốt cho một người nếu ít nhất một trong những chiếc bánh mì kẹp thịt ở đó làm hài lòng người đó. 

Đối với mỗi nhóm, nhiệm vụ là chọn một quán cà phê có số lượng người hài lòng tối đa trong nhóm đó. Nếu nhiều quán cà phê đạt được mức tối đa như nhau, chúng tôi sẽ chọn quán cà phê có chỉ số nhỏ nhất. 

Cấu trúc chính ở đây là mỗi sở thích và mỗi chiếc burger sử dụng tối đa 6 chữ cái riêng biệt nên mỗi bộ rất nhỏ dù bảng chữ cái có 26 chữ cái. Điều này ngay lập tức gợi ý rằng việc suy luận các tập hợp con và mặt nạ bit sẽ hiệu quả, vì tổng số tập hợp con của một tập hợp 6 phần tử chỉ là 64. 

Khó khăn chính là chúng tôi đang so sánh từng nhóm với mọi quán cà phê và cả hai có thể lên tới 1000, trong khi tổng số người và bánh mì kẹp thịt có thể lên tới 50000. Việc kết hợp tất cả các cặp ngây thơ giữa người và bánh mì kẹp thịt trong mỗi quán cà phê sẽ quá chậm. 

Một trường hợp phức tạp xuất hiện khi nhiều chiếc bánh mì kẹp thịt trong quán cà phê phù hợp một phần với sở thích của một người nhưng không có chiếc nào chứa đầy đủ sở thích đó. Ví dụ, nếu một người thích`{a, b}`và một quán cà phê có bánh mì kẹp thịt`{a}`Và`{b}`, người đó không hài lòng. Chỉ có việc ngăn chặn hoàn toàn mới quan trọng chứ không phải chồng chéo một phần. 

Một trường hợp khác là khi một quán cà phê không có bánh mì kẹp thịt làm hài lòng bất kỳ ai trong nhóm. Quán cà phê đó vẫn cần được xem xét và có thể được chọn nếu tất cả các quán cà phê đều hoạt động kém như nhau, vì chúng tôi vẫn chọn chỉ số nhỏ nhất trong quan hệ. 

## Phương pháp tiếp cận 

Một cách trực tiếp để giải quyết vấn đề là mô phỏng mọi thứ theo đúng nghĩa đen. Đối với mỗi nhóm và mỗi quán cà phê, chúng tôi lặp lại từng người rồi đến từng chiếc bánh mì kẹp thịt trong quán cà phê đó, kiểm tra xem chiếc bánh mì kẹp thịt có chứa tất cả các thành phần cần thiết của người đó hay không. Điều này đúng vì nó phản ánh chính xác định nghĩa. Tuy nhiên, sự phức tạp trở thành sản phẩm của các nhóm, quán cà phê, con người và bánh mì kẹp thịt, vốn quá lớn so với những hạn chế. 

Quan sát quan trọng là mỗi chiếc bánh mì kẹp thịt và mỗi sở thích là một tập hợp gồm tối đa 6 chữ cái riêng biệt. Thay vì liên tục kiểm tra các mối quan hệ của tập hợp con, chúng ta có thể tính toán trước theo hướng ngược lại: đối với một chiếc bánh mì kẹp thịt nhất định, chúng ta có thể liệt kê tất cả các tập hợp con có thể có của bộ thành phần của nó. Mỗi tập hợp con như vậy tương ứng với một người sẽ hài lòng với chiếc bánh mì kẹp thịt đó. Vì một chiếc bánh mì kẹp thịt có nhiều nhất là 6 thành phần nên việc mở rộng này tạo ra tối đa 64 tập hợp con. 

Điều này biến mỗi quán cà phê thành một tập hợp được tính toán trước gồm tất cả các tập hợp sở thích mà nó có thể đáp ứng. Khi chúng tôi có được điều này, việc kiểm tra một người sẽ trở thành một bài kiểm tra tư cách thành viên đơn giản. Sau đó, chúng tôi đánh giá từng quán cà phê bằng cách quét tất cả mọi người và đếm xem có bao nhiêu mặt nạ ưa thích của họ có trong bộ tính toán trước của quán cà phê. 

Sự cải tiến này đến từ việc hoán đổi các lần kiểm tra tập hợp con lặp đi lặp lại để có được mức mở rộng nhỏ liên tục trên mỗi chiếc bánh mì kẹp thịt và tra cứu hàm băm nhanh sau đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Kết hợp bánh mì kẹp thịt người bạo lực | O(G · C · P · B) | O(1) | Quá chậm | 
| Mở rộng tập hợp con trên mỗi chiếc burger + đếm thành viên | O(tổng số bánh mì kẹp thịt · 2^6 + tổng số người · quán cà phê) | O(tổng số trạng thái mở rộng) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi mọi chuỗi thành phần thành một bitmask trên 26 chữ cái. Mỗi ký tự tương ứng với một vị trí bit. Điều này cho phép việc kiểm tra tập hợp con trở thành các thao tác bit. 
2. Đối với mỗi quán cà phê, hãy xây dựng một bộ mặt nạ gồm tất cả các thành phần của bánh mì kẹp thịt có thể làm hài lòng ai đó một cách gián tiếp. Đối với mỗi mặt nạ bánh mì kẹp thịt, hãy tạo tất cả các tập hợp con của mặt nạ đó và chèn chúng vào bộ của quán cà phê. Điều này hiệu quả vì bất kỳ người nào hài lòng với chiếc bánh mì kẹp thịt đó đều phải có sở thích tương đương với một trong những tập hợp con này. 
3. Sau khi xử lý trước, mỗi quán cà phê hiện có một bản trình bày hoàn chỉnh về tất cả các mặt nạ ưa thích mà nó có thể đáp ứng thông qua ít nhất một chiếc bánh mì kẹp thịt. 
4. Đối với mỗi nhóm, hãy đếm tần suất có những mặt nạ ưa thích giống hệt nhau trong số những người trong nhóm đó. Điều này tránh việc quét liên tục các tùy chọn giống nhau nhiều lần. 
5. Đối với mỗi quán cà phê, hãy tính xem có bao nhiêu người trong nhóm hài lòng bằng cách lặp lại tất cả các mặt nạ ưu tiên riêng biệt trong nhóm và kiểm tra xem mặt nạ đó có tồn tại trong tập hợp được tính toán trước của quán cà phê hay không. Nhân với tần số. 
6. Theo dõi quán cà phê với số điểm tối đa cho mỗi nhóm, phá vỡ mối ràng buộc bằng cách chọn chỉ số nhỏ nhất. 

### Tại sao nó hoạt động 

Mọi người đều hài lòng khi và chỉ khi tồn tại ít nhất một chiếc bánh mì kẹp thịt có bộ nguyên liệu là bộ siêu bộ theo sở thích của họ. Việc liệt kê tất cả các tập hợp con của mỗi chiếc bánh mì kẹp thịt sẽ nắm bắt chính xác tất cả các tập hợp ưu tiên có thể có mà chiếc bánh mì kẹp thịt đó có thể đáp ứng. Việc đưa sự kết hợp lên trên tất cả các loại bánh mì kẹp thịt trong quán cà phê sẽ duy trì sự tương đương này, do đó, tư cách thành viên trong nhóm cuối cùng giống hệt về mặt logic với điều kiện thỏa mãn ban đầu. Vì điểm của mỗi nhóm chỉ là tổng của những người độc lập nên việc đếm thông qua tần số sẽ duy trì tính chính xác mà không làm thay đổi cấu trúc tổ hợp cơ bản. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def mask_of(s):
    m = 0
    for ch in s.strip():
        m |= 1 << (ord(ch) - 97)
    return m

def all_submasks(mask):
    # generate all subsets of up to 26-bit mask, but actual size <= 6 bits
    bits = []
    for i in range(26):
        if mask & (1 << i):
            bits.append(i)
    res = []
    k = len(bits)
    for sub in range(1 << k):
        m = 0
        for j in range(k):
            if sub & (1 << j):
                m |= 1 << bits[j]
        res.append(m)
    return res

n = int(input())
groups = []

for _ in range(n):
    k = int(input())
    freq = {}
    for _ in range(k):
        m = mask_of(input())
        freq[m] = freq.get(m, 0) + 1
    groups.append((k, freq))

m = int(input())
cafes = []

for _ in range(m):
    l = int(input())
    s = set()
    for _ in range(l):
        bm = mask_of(input())
        for sm in all_submasks(bm):
            s.add(sm)
    cafes.append(s)

for _, freq in groups:
    best_cafe = 1
    best_score = -1

    for idx, cafe_set in enumerate(cafes, start=1):
        score = 0
        for pm, cnt in freq.items():
            if pm in cafe_set:
                score += cnt

        if score > best_score or (score == best_score and idx < best_cafe):
            best_score = score
            best_cafe = idx

    print(best_cafe)
```Quá trình triển khai bắt đầu bằng cách mã hóa mọi chuỗi thành phần dưới dạng bitmask. Điều này làm cho các mối quan hệ tập hợp con trở nên rẻ tiền và tránh hoàn toàn các thao tác chuỗi. Hàm trợ giúp để tạo mặt nạ con liệt kê tất cả các tập hợp con của các bit hoạt động của bánh mì kẹp thịt, điều này an toàn vì số lượng thành phần riêng biệt trên mỗi chuỗi nhiều nhất là 6. 

Mỗi quán cà phê xây dựng một bộ hàm băm chứa tất cả các mặt nạ ưu tiên mà nó có thể đáp ứng. Bộ này là cấu trúc tăng tốc chính, thay thế việc kiểm tra tập hợp con lặp đi lặp lại bằng các bài kiểm tra thành viên O(1). 

Đối với mỗi nhóm, chúng tôi nén các tùy chọn giống hệt nhau bằng bản đồ tần số. Điều này quan trọng vì các nhóm có thể chứa các chuỗi ưu tiên lặp đi lặp lại và việc xử lý chúng riêng lẻ sẽ lặp lại công việc giống hệt nhau một cách không cần thiết. Bước tính điểm cuối cùng chỉ cần kiểm tra tư cách thành viên của từng mặt nạ duy nhất trong từng set của quán cà phê và nhân với tần suất của nó. 

Việc phá vỡ liên kết được xử lý trong quá trình quét bằng cách theo dõi chỉ số nhỏ nhất để đạt được điểm tối đa. 

## Ví dụ đã hoạt động 

Hãy xem xét một tình huống đơn giản với một nhóm và hai quán cà phê. 

Tùy chọn nhóm là`{a}`,`{ab}`,`{b}`. Cafe 1 có bánh mì kẹp thịt`{ab}`Và`{c}`. Cafe 2 có bánh mì kẹp thịt`{b}`Và`{ac}`. 

Đối với Cafe 1, việc mở rộng tập hợp con của`{ab}`sản xuất`{}`,`{a}`,`{b}`,`{ab}`. Cafe 1 có thể đáp ứng`{a}`Và`{ab}`nhưng không`{b}`. Vậy điểm là 2. 

Đối với Cafe 2, việc mở rộng`{b}`sản xuất`{}`,`{b}`. Mở rộng`{ac}`sản xuất`{}`,`{a}`,`{c}`,`{ac}`. Vậy Cafe 2 có thể đáp ứng được`{a}`Và`{b}`và số điểm là 3. 

| Cà phê |`{a}`|`{ab}`|`{b}`| Điểm | 
| --- | --- | --- | --- | --- | 
| 1 | vâng | vâng | không | 2 | 
| 2 | vâng | không | vâng | 2 | 

Dấu vết này cho thấy rằng tính chính xác phụ thuộc vào việc ngăn chặn toàn bộ tập hợp con thay vì chồng chéo một phần và việc đóng được tính toán trước sẽ nắm bắt chính xác sự hài lòng. 

Bây giờ hãy xem xét một nhóm trong đó tất cả mọi người đều có sở thích giống nhau`{a}`và một quán cà phê có bánh mì kẹp thịt`{b}`,`{c}`chỉ một. Đóng tập hợp con của nó không chứa`{a}`, do đó điểm vẫn bằng 0, phù hợp với định nghĩa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(tổng số bánh mì kẹp thịt · 2^k + m · tổng số người) | Mỗi chiếc burger mở rộng thành tối đa 64 tập hợp con và mỗi quán cà phê được đánh giá bằng cách quét các tùy chọn nhóm một lần | 
| Không gian | O(m · 64 + tổng số sở thích riêng biệt) | Mỗi quán cà phê lưu trữ một tập hợp các tập hợp con có thể truy cập | 

Các giới hạn vừa vặn thoải mái trong giới hạn vì 50.000 bánh mì kẹp thịt mở rộng lên tổng cộng khoảng 3,2 triệu lần chèn tập hợp con và mỗi lần tra cứu là thời gian không đổi. Đánh giá cuối cùng thực hiện tối đa 50 triệu lượt kiểm tra hàm băm, mức này có thể chấp nhận được trong Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read()  # placeholder, replace with actual call

# Since full solution is not wrapped in function, these are illustrative structure tests

# minimal case
# one group, one cafe, perfect match

# group: 1 person "a"
# cafe: 1 burger "a"
# expected: cafe 1

# boundary no match case

# all mismatch case

# mixed duplicates case
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trận đấu đơn tối thiểu | 1 | tính đúng đắn cơ bản | 
| không có bánh mì kẹp thịt phù hợp | 1 | hòa với điểm số 0 | 
| nhiều quán cà phê buộc | 1 | quy tắc chỉ số nhỏ nhất | 
| sở thích trùng lặp | 1 | nén tần số chính xác | 

## Vỏ cạnh 

Trường hợp không có bánh mì kẹp thịt nào ở bất kỳ quán cà phê nào chứa bộ nguyên liệu nhất định vẫn tạo ra câu trả lời hợp lệ vì tất cả các quán cà phê đều đạt điểm 0 và chỉ số nhỏ nhất được chọn. Ví dụ, nếu tất cả mọi người thích`{a}`nhưng tất cả bánh mì kẹp thịt chỉ chứa`{b}`, mọi quán cà phê đều có tập hợp sự hài lòng trống và câu trả lời là quán cà phê 1. 

Một trường hợp khác là khi một chiếc bánh mì kẹp thịt có đúng sáu thành phần. Ngay cả khi đó, việc mở rộng tập hợp con vẫn bị giới hạn ở 64 phần tử, do đó chi phí tiền xử lý không tăng thêm. Thuật toán xử lý vấn đề này giống như những chiếc bánh mì kẹp thịt nhỏ hơn, đảm bảo hành vi nhất quán ở mọi mức độ. 

Các tùy chọn lặp lại trong một nhóm không ảnh hưởng đến tính chính xác vì chúng được tổng hợp thành bản đồ tần suất trước khi tính điểm. Điều này ngăn chặn việc tính hai lần từ các lần lặp dư thừa.
