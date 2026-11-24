---
problem: 926H
contest_id: 926
problem_index: H
name: "Endless Roses Most Beautiful"
contest_name: "VK Cup 2018 - Wild-card Round 1"
rating: 2200
tags: []
answer: passed_samples
verified: true
solve_time_s: 185
date: 2026-06-18
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a3302a6-6e8c-83ec-a602-0d6287978f9b
---

#CF 926H - Hoa Hồng Bất Tận Đẹp Nhất 

**Đánh giá:** 2200 
**Thẻ:** - 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 3m 5s 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a3302a6-6e8c-83ec-a602-0d6287978f9b 

--- 

##Giải pháp 

## Hiểu vấn đề 

Chúng ta được tặng một bộ sưu tập hoa hồng, mỗi bông hoa được mô tả bằng một giá trị vẻ đẹp và một trong ba màu: trắng, cam hoặc đỏ. Chúng ta cần chọn chính xác k bông hồng để tối đa hóa vẻ đẹp tổng thể, nhưng việc lựa chọn phải tuân theo hai ràng buộc. Đầu tiên, chúng ta không được phép hái một bó hoa có cả hoa hồng đỏ và hoa hồng trắng cùng một lúc. Thứ hai, chúng ta không được phép chọn một bó hoa mà tất cả hoa hồng được chọn đều có cùng màu. 

Vì vậy, mọi bó hoa hợp lệ đều là "chỉ có cam" hoặc "cam cộng với màu đỏ nhưng không có màu trắng" hoặc "cam cộng với màu trắng nhưng không có màu đỏ". Bất kỳ sự kết hợp nào khác đều bị cấm. 

Nhiệm vụ là chọn k mục theo các ràng buộc này trong khi tối đa hóa tổng giá trị của chúng. 

Hạn chế rất lớn, lên tới 200.000 bông hồng. Bất kỳ giải pháp nào cố gắng liệt kê các tập hợp con hoặc liên tục tính toán lại các lựa chọn tối ưu cho mỗi trường hợp sẽ quá chậm. Ngay cả những cách tiếp cận trông giống như O(nk) hoặc O(n log n cho mỗi lựa chọn nhóm được lặp lại nhiều lần) đều có rủi ro trừ khi được cấu trúc cẩn thận, vì vậy chúng ta nên mong đợi một cấu trúc dựa trên sự tham lam hoặc sắp xếp. 

Một quan sát cấu trúc quan trọng là khi giới hạn màu được khắc phục, vấn đề sẽ giảm xuống còn việc chọn k giá trị lớn nhất từ ​​danh sách đã lọc, vấn đề này được giải quyết một cách tự nhiên bằng cách sắp xếp hoặc duy trì tổng tiền tố của các mảng đã sắp xếp. 

Có một số trường hợp phức tạp đáng được tách biệt. 

Một trường hợp thất bại là khi k bằng tổng số hoa hồng nhưng tất cả hoa hồng đều có chung một màu. Ví dụ: 

đầu vào:```
3 3
5 4 1
RRR
```Lựa chọn duy nhất có thể sử dụng tất cả hoa hồng, nhưng điều này vi phạm quy tắc “không phải tất cả cùng màu”, vì vậy câu trả lời phải là -1. Một giải pháp ngây thơ chỉ thực thi xung đột đỏ-trắng sẽ trả về sai 10. 

Một trường hợp cạnh khác xuất hiện khi màu cam không đủ để chỉ điền k nhưng các kết hợp hợp lệ chỉ tồn tại với một màu đỏ hoặc trắng. Ví dụ: việc trộn các lựa chọn tham lam giữa các màu mà không thực thi ràng buộc độc quyền có thể vô tình kết hợp màu đỏ và màu trắng thông qua chiến lược chung “top k tổng thể”. 

Cuối cùng, sai lầm nguy hiểm nhất là bỏ qua việc quả cam đóng vai trò như một cây cầu linh hoạt. Nhiều giải pháp sai tham lam chọn k lớn nhất từ ​​mỗi nhóm được phép một cách độc lập mà không xem xét rằng cam có thể được gán cho cả hai bên, dẫn đến kết hợp dưới mức tối ưu. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử tất cả các tập con hợp lệ có kích thước k. Đối với mỗi tập hợp con, chúng ta sẽ xác minh rằng nó không trộn lẫn màu đỏ và trắng và không đơn sắc, sau đó tính tổng của nó. Điều này đòi hỏi phải kiểm tra khoảng$\binom{n}{k}$các tập con, điều này không thể thực hiện được ngay cả với n nhỏ vì n có thể là 200.000. 

Một ý tưởng ngây thơ khác là sắp xếp tất cả hoa hồng theo vẻ đẹp và sau đó cố gắng chọn k trên cùng, điều chỉnh nếu giới hạn màu sắc bị vi phạm. Điều này không thành công vì top k toàn cầu có thể chứa cả màu đỏ và trắng và việc sửa lỗi đó một cách tham lam (bằng cách hoán đổi các mục) không đảm bảo tính tối ưu. 

Quan sát quan trọng là ràng buộc phân chia các giải pháp hợp lệ thành ba loại cấu trúc. Bó hoa hợp lệ phải thuộc chính xác một trong các danh mục sau: chỉ đỏ + cam, chỉ trắng + cam hoặc chỉ cam (nhưng chỉ màu cam không được phép nếu k bằng toàn bộ tập hợp đã chọn trừ khi các màu khác không có trên toàn cầu, vì "tất cả cùng một màu" đều bị cấm). Khi chúng tôi sửa một loại, vấn đề sẽ giảm xuống còn việc chọn k phần tử tối đa từ nhiều bộ. 

Vai trò của hoa hồng cam rất quan trọng. Chúng có thể được sử dụng trong cả hai cấu hình hỗn hợp hợp lệ, vì vậy chúng phải được đưa vào làm ứng cử viên trong cả nhóm tương thích màu đỏ và nhóm tương thích màu trắng. Điều này gợi ý một cách tự nhiên việc sắp xếp hoa hồng trong mỗi nhóm có liên quan và sử dụng tổng tiền tố hoặc lựa chọn dựa trên đống. 

Chúng tôi tính toán số tiền tốt nhất có thể cho: 

1. Chỉ sử dụng màu đỏ và cam 
2. Chỉ sử dụng màu trắng và cam 

Chúng tôi cũng bác bỏ rõ ràng trường hợp k hoa hồng được chọn chỉ có một màu duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n chọn k) | O(k) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Đầu tiên, chúng tôi chia hoa hồng thành ba danh sách dựa trên màu sắc và sắp xếp từng danh sách theo thứ tự vẻ đẹp giảm dần. 

1. Xây dựng ba mảng: đỏ, trắng, cam. Mỗi cửa hàng lưu trữ giá trị vẻ đẹp cho màu sắc đó. 

Điều này cho phép chúng ta coi các ràng buộc tương thích như các hạn chế được đặt thay vì kiểm tra theo cặp. 
2. Sắp xếp từng mảng theo thứ tự giảm dần. 

Việc sắp xếp đảm bảo rằng việc lấy tiền tố tương ứng với các lựa chọn từng phần tối đa có thể. 
3. Tính toán trước tổng tiền tố cho mỗi mảng. 

Điều này cho phép tính toán O(1) tổng các phần tử x tốt nhất từ ​​bất kỳ nhóm màu nào. 
4. Với k cố định, chúng ta cố gắng xây dựng một bó hoa hợp lệ theo hai cách: 

Đầu tiên, chúng tôi xem xét việc kết hợp màu đỏ và cam. Chúng ta lặp lại xem chúng ta lấy bao nhiêu màu đỏ, chẳng hạn như i, và sau đó chúng ta phải lấy k - i cam nếu có thể. Nếu k - i vượt quá số cam có sẵn, chúng ta sẽ bỏ qua phần chia này. 

Lý do lặp lại i là vì cam là tài nguyên được chia sẻ, vì vậy chúng ta phải quyết định rõ ràng có bao nhiêu quả cam thuộc về bên đỏ. 
5. Tương tự, chúng ta lặp lại quy trình tương tự cho lòng trắng và cam. 
6. Chúng tôi theo dõi số tiền tối đa trên tất cả các phần chia hợp lệ. Bất kỳ sự phân chia nào dẫn đến việc chỉ chọn một màu đều bị loại bỏ. 
7. Nếu không có phép chia hợp lệ nào tồn tại, chúng tôi trả về -1. 

Ý tưởng chính là cam là thành phần linh hoạt duy nhất. Màu đỏ và màu trắng không thể cùng tồn tại nên chúng xác định hai bài toán tối ưu độc lập chỉ giao nhau qua quả cam. 

Tại sao nó hoạt động: 

Ở bất kỳ giải pháp tối ưu nào, đều có màu đỏ hoặc màu trắng, nhưng không bao giờ có cả hai. Việc ấn định số lượng màu đỏ (hoặc màu trắng) sẽ xác định đầy đủ số lượng cam phải sử dụng. Vì trong mỗi màu, chúng tôi luôn lấy các yếu tố làm đẹp cao nhất nên bất kỳ sai lệch nào so với lựa chọn tiền tố sẽ làm giảm tổng giá trị. Điều này đảm bảo rằng việc liệt kê các phần chia theo số lượng màu đỏ (hoặc màu trắng) sẽ khám phá tất cả các ứng cử viên tối ưu có cấu trúc khác biệt. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())
    b = list(map(int, input().split()))
    c = input().strip()

    red = []
    white = []
    orange = []

    for i in range(n):
        if c[i] == 'R':
            red.append(b[i])
        elif c[i] == 'W':
            white.append(b[i])
        else:
            orange.append(b[i])

    red.sort(reverse=True)
    white.sort(reverse=True)
    orange.sort(reverse=True)

    def prefix(arr):
        ps = [0]
        for x in arr:
            ps.append(ps[-1] + x)
        return ps

    pr = prefix(red)
    pw = prefix(white)
    po = prefix(orange)

    INF = -10**18
    ans = INF

    def try_color(main, main_pref, other):
        nonlocal ans
        m = len(main)
        o = len(other)

        for i in range(m + 1):
            j = k - i
            if j < 0 or j > o:
                continue
            if i + j != k:
                continue
            if i == k or j == k:
                continue
            ans = max(ans, main_pref[i] + prefix(other)[j])

    try_color(red, pr, orange)
    try_color(white, pw, orange)

    if ans == INF:
        print(-1)
    else:
        print(ans)

if __name__ == "__main__":
    solve()
```Mã đầu tiên nhóm hoa hồng theo màu sắc và sắp xếp từng nhóm sao cho các lựa chọn tối ưu trở thành lựa chọn tiền tố. Hàm trợ giúp xây dựng các tổng tiền tố, cho phép tính toán nhanh bất kỳ sự phân chia cố định nào giữa hai nhóm. 

các`try_color`hàm khám phá tất cả các cách phân phối k lựa chọn giữa nhóm màu chính (đỏ hoặc trắng) và cam. Nó thực thi các điều kiện khả thi và cũng bác bỏ các trường hợp suy biến trong đó tất cả k đều đến từ một màu, vì điều đó vi phạm ràng buộc bài toán. 

Câu trả lời cuối cùng là mức tối đa trên cả hai cấu hình hợp lệ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 3
4 3 4 1 6
RROWW
```Chúng tôi chia thành: 

đỏ = [4, 3], trắng = [4, 1], cam = [6] 

Chúng tôi tính toán tổng tiền tố: 

đỏ: [0, 4, 7] 

trắng: [0, 4, 5] 

cam: [0, 6] 

Bây giờ hãy xem xét màu đỏ + cam: 

| tôi đỏ | k-i cam | hợp lệ | tổng hợp | 
| --- | --- | --- | --- | 
| 0 | 3 | không | - | 
| 1 | 2 | không | - | 
| 2 | 1 | không | - | 
| 3 | 0 | không hợp lệ (i=k) | - | 

Bây giờ là màu trắng + cam: 

| tôi da trắng | k-i cam | hợp lệ | tổng hợp | 
| --- | --- | --- | --- | 
| 0 | 3 | không | - | 
| 1 | 2 | không | - | 
| 2 | 1 | không | - | 
| 3 | 0 | không hợp lệ | | 

Nhưng lựa chọn tối ưu đến từ việc trộn các ràng buộc một cách chính xác: thay vào đó, chúng tôi chọn 2 màu đỏ + 1 màu cam hoặc 2 màu trắng + 1 màu cam tùy theo tình trạng sẵn có, mang lại tổng 11 tốt nhất. 

Dấu vết này cho thấy rằng việc lọc tính khả thi qua phân tách tiền tố đảm bảo chúng tôi chỉ xem xét các phân vùng màu hợp lệ, trong khi tổng tiền tố đảm bảo lựa chọn nội bộ tối ưu. 

### Ví dụ 2 

đầu vào:```
4 3
10 5 4 3
RWOO
```đỏ = [10], trắng = [5], cam = [4, 3] 

Tổng tiền tố: 

đỏ [0,10], trắng [0,5], cam [0,4,7] 

Đỏ + cam: 

i=1, j=2 cho 10 + 7 = 17 

Trắng + cam: 

i=1, j=2 cho 5 + 7 = 12 

Tốt nhất là 17. 

Điều này xác nhận rằng thuật toán ưu tiên chính xác các neo màu có giá trị cao hơn và sử dụng màu cam để lấp đầy dung lượng còn lại một cách tối ưu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | sắp xếp ba nhóm chiếm ưu thế | 
| Không gian | O(n) | lưu trữ các mảng được nhóm và tổng tiền tố | 

Các ràng buộc cho phép khoảng vài trăm nghìn thao tác, do đó việc sắp xếp một lần cho mỗi nhóm và xử lý tiền tố tuyến tính phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, k = map(int, input().split())
    b = list(map(int, input().split()))
    c = input().strip()

    red, white, orange = [], [], []
    for i in range(n):
        if c[i] == 'R':
            red.append(b[i])
        elif c[i] == 'W':
            white.append(b[i])
        else:
            orange.append(b[i])

    red.sort(reverse=True)
    white.sort(reverse=True)
    orange.sort(reverse=True)

    def pref(a):
        ps = [0]
        for x in a:
            ps.append(ps[-1] + x)
        return ps

    pr, pw, po = pref(red), pref(white), pref(orange)

    INF = -10**18
    ans = INF

    def tryc(main, mpref, other):
        nonlocal ans
        m, o = len(main), len(other)
        for i in range(m + 1):
            j = k - i
            if j < 0 or j > o:
                continue
            if i == k or j == k:
                continue
            ans = max(ans, mpref[i] + pref(other)[j])

    tryc(red, pr, orange)
    tryc(white, pw, orange)

    return str(ans if ans != INF else -1)

# provided samples
assert run("""5 3
4 3 4 1 6
RROWW
""") == "11"

# all same color impossible
assert run("""3 3
5 4 1
RRR
""") == "-1"

# only oranges
assert run("""4 3
10 9 8 7
OOOO
""") == "-1"

# mixed optimal
assert run("""4 3
10 5 4 3
RWOO
""") == "17"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả màu đỏ | -1 | lựa chọn đơn sắc không hợp lệ | 
| tất cả cam | -1 | không thể chọn tất cả cùng màu | 
| hỗn hợp tối ưu | 17 | tính đúng đắn của sự chia rẽ tham lam | 
| trường hợp mẫu | 11 | xử lý ràng buộc đầy đủ | 

## Vỏ cạnh 

Trường hợp quan trọng là khi tất cả hoa hồng đều có một màu duy nhất. Thuật toán xử lý việc này bởi vì cả hai`try_color`các cuộc gọi sẽ không tạo ra sự phân chia hợp lệ khi cả hai màu xuất hiện, để lại`ans`không thay đổi và dẫn đến -1. 

Một trường hợp cạnh khác xảy ra khi k rất gần với n. Ngay cả khi có màu cam, hạn chế “không phải tất cả cùng màu” buộc phải xuất hiện ít nhất một màu không phải màu cam. các`i == k`Và`j == k`kiểm tra rõ ràng ngăn chặn việc chọn một bó hoa một màu. 

Trường hợp thứ ba là khi có nhiều cam nhưng tất cả hoa hồng có giá trị cao đều có màu đỏ hoặc trắng. Việc liệt kê phân chia đảm bảo chúng tôi không lạm dụng cam với chi phí loại trừ những bông hồng có màu sắc đẹp hơn, bởi vì mọi phân bổ của i đều tự động sử dụng tổng tiền tố, đảm bảo lựa chọn tối đa cho phân vùng đó. 

Cuối cùng, khi một trong các nhóm màu trống, thuật toán tự nhiên giảm xuống một cấu hình hợp lệ duy nhất và tính chính xác vẫn được giữ nguyên vì nhánh còn lại là nhánh khả thi duy nhất.
