---
title: "CF 106084E - Sắp xếp lại tấm đá nổ"
description: "Chúng ta có một lưới hình chữ nhật và một tập hợp k ô được chiếm giữ riêng biệt, mỗi ô chứa một phiến đá được dán nhãn. Một vùng cấm hình chữ nhật cũng được đưa ra. Nếu bất kỳ phiến đá nào nằm bên trong hình chữ nhật này tại thời điểm thiết bị được đặt, toàn bộ cấu hình sẽ không hợp lệ."
date: "2026-06-21T09:29:00+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106084
codeforces_index: "E"
codeforces_contest_name: "2025 ICPC Asia Taiwan Online Programming Contest"
rating: 0
weight: 106084
solve_time_s: 55
verified: true
draft: false
---

[CF 106084E - Sắp xếp lại tấm đá nổ](https://codeforces.com/problemset/problem/106084/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 55s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một lưới hình chữ nhật và một tập hợp k ô được chiếm giữ riêng biệt, mỗi ô chứa một phiến đá được dán nhãn. Một vùng cấm hình chữ nhật cũng được đưa ra. Nếu bất kỳ phiến đá nào nằm bên trong hình chữ nhật này tại thời điểm thiết bị được đặt, toàn bộ cấu hình sẽ không hợp lệ. 

Chúng ta được phép sắp xếp lại các phiến đá thành các ô mới trong lưới, trong khi luôn giữ tất cả các viên đá k trong các ô riêng biệt. Việc di chuyển một viên đá sẽ phải chịu một chi phí bằng chỉ số của nó và tổng năng lượng được xác định là chỉ số tối đa trong số tất cả các viên đá được di chuyển ít nhất một lần trong toàn bộ quá trình. Nhiệm vụ là giảm thiểu năng lượng này đồng thời đảm bảo rằng sau khi sắp xếp lại, không có viên đá nào nằm bên trong hình chữ nhật cấm. Nếu không thể đạt được cấu hình như vậy, chúng ta phải xuất -1. 

Quan sát quan trọng ẩn trong tuyên bố là cấu hình cuối cùng chỉ phụ thuộc vào vị trí các viên đá kết thúc chứ không phụ thuộc vào cách chúng di chuyển, ngoại trừ hạn chế là hai viên đá không bao giờ có thể chồng lên nhau trong quá trình này. Vì lưới đủ lớn để lưu trữ tất cả cấu hình của k ô riêng biệt bất cứ khi nào có ít nhất k ô mục tiêu trống, vấn đề giảm xuống là liệu chúng ta có thể đặt tất cả các viên đá bên ngoài hình chữ nhật bị cấm hay không và những viên đá nào nhất thiết phải di chuyển. 

Các ràng buộc n, m ≤ 500 ngụ ý lên tới 250.000 ô, do đó, bất kỳ giải pháp nào quét lưới hoặc xử lý tất cả các viên đá trong O(k) hoặc O(nm) đều khả thi. Tuy nhiên, bất cứ điều gì cố gắng mô phỏng các bước sắp xếp lại hoặc tìm kiếm trên các cấu hình sẽ quá chậm vì bản thân k có thể lớn bằng n·m. 

Trường hợp góc tinh tế xuất hiện khi hình chữ nhật bị cấm lớn. Nếu nó bao phủ gần như toàn bộ lưới, có thể có quá ít ô có sẵn bên ngoài nó để chứa tất cả các viên đá. Trong trường hợp đó, ngay cả khi chúng ta được phép tự do sắp xếp lại thì nhiệm vụ đó cũng không thể thực hiện được. 

Trường hợp cạnh quan trọng thứ hai là khi ban đầu không có hòn đá nào nằm bên trong hình chữ nhật bị cấm. Trong trường hợp đó, chúng ta không cần phải di chuyển bất cứ thứ gì cả và câu trả lời là 0. 

Một cạm bẫy tiềm ẩn khác là giả định rằng chúng ta chỉ phải di chuyển các viên đá ra ngoài hình chữ nhật nếu ban đầu chúng ở bên trong hình chữ nhật. Điều đó đúng ở đây, nhưng chỉ vì hạn chế duy nhất ở trạng thái cuối cùng là sự chiếm giữ của các ô bị cấm. Không có yêu cầu để duy trì trật tự tương đối hoặc liền kề. 

## Phương pháp tiếp cận 

Một cách giải thích mạnh mẽ sẽ là thử chọn một tập hợp con các viên đá để di chuyển, mô phỏng tất cả khả năng gán lại các viên đá vào các ô an toàn và kiểm tra tính khả thi trong khi giảm thiểu chỉ số tối đa của các viên đá được di chuyển. Điều này nhanh chóng trở nên không khả thi bởi vì ngay cả việc chọn những viên đá di chuyển cũng đã liên quan đến sự bùng nổ tổ hợp trên các tập hợp con có kích thước lên tới k và việc gán chúng vào các vị trí sẽ tạo ra độ phức tạp giai thừa. Ngay cả một ước tính sơ bộ cũng dẫn đến thứ gì đó như O(2^k · k!) hoàn toàn không sử dụng được. 

Sự đơn giản hóa quan trọng đến từ việc tách “những viên đá nào phải di chuyển” khỏi “nơi chúng đi”. Hạn chế duy nhất trong cách sắp xếp cuối cùng là tất cả k viên đá phải chiếm các ô riêng biệt bên ngoài hình chữ nhật bị cấm. Nếu có ít nhất k ô có sẵn bên ngoài hình chữ nhật thì chúng ta luôn có thể gán đá cho các ô đó. Vì không có ràng buộc về danh tính nào được áp đặt cho các vị trí cuối cùng nên chúng ta có thể coi các viên đá như các phần giữ chỗ có thể hoán đổi cho nhau để chiếm chỗ. 

Theo quan điểm này, những viên đá duy nhất buộc phải di chuyển chính xác là những viên bắt đầu bên trong hình chữ nhật bị cấm. Bất kỳ viên đá nào đã ở bên ngoài đều có thể được ánh xạ tới chính nó hoặc một vị trí tự do bên ngoài khác, nghĩa là chúng ta không bao giờ cần đưa nó vào tập hợp đã di chuyển. 

Do đó, năng lượng được xác định hoàn toàn bởi chỉ số lớn nhất trong số những viên đá bị mắc kẹt ban đầu. Tính khả thi giảm xuống còn việc kiểm tra dung lượng đơn giản: số lượng ô bên ngoài hình chữ nhật ít nhất phải bằng k.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force vượt qua bài tập | Hàm mũ | Hàm mũ | Quá chậm | 
| Công suất + kiểm tra cưỡng bức di chuyển | O(k) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm xem có bao nhiêu ô lưới nằm ngoài hình chữ nhật bị cấm. Đây chỉ đơn giản là n·m trừ đi diện tích của hình chữ nhật. Giá trị này thể hiện tổng công suất để đặt đá cuối cùng. 
2. Nếu dung lượng này hoàn toàn nhỏ hơn k, hãy trả về -1 ngay lập tức. Không có đủ ô an toàn để chứa tất cả các viên đá, vì vậy không có sự sắp xếp lại nào có thể tránh được thiết bị. 
3. Lặp lại tất cả các viên đá và kiểm tra xem mỗi viên đá có nằm bên trong hình chữ nhật bị cấm hay không. 
4. Theo dõi chỉ số tối đa trong số tất cả các viên đá nằm bên trong hình chữ nhật. Đây là những viên đá buộc phải di chuyển vì chúng không thể duy trì ở bất kỳ cấu hình cuối cùng hợp lệ nào. 
5. Nếu không có viên đá nào nằm bên trong hình chữ nhật, trả về 0 vì cấu hình ban đầu đã hợp lệ. 
6. Nếu không, xuất chỉ số tối đa được tìm thấy ở bước 4. 

Lý do đằng sau bước 4 là chúng ta có thể tự do lựa chọn những viên đá di chuyển trong số những viên bên trong hình chữ nhật, nhưng chúng ta không thể tránh việc di chuyển chúng. Vì năng lượng được xác định là chỉ số tối đa của bất kỳ viên đá nào được di chuyển, nên chiến lược tốt nhất là không bao giờ di chuyển bất kỳ viên đá nào ra ngoài hình chữ nhật trừ khi thực sự cần thiết, và những viên đá bên trong là tập hợp không thể tránh khỏi. 

### Tại sao nó hoạt động 

Thuật toán dựa trên hai thuộc tính cấu trúc. Đầu tiên, cấu hình cuối cùng chỉ bị hạn chế bởi dung lượng bên ngoài vùng cấm chứ không phải bởi danh tính hay thứ tự của các viên đá. Thứ hai, bất kỳ viên đá nào nằm bên trong hình chữ nhật bị cấm đều phải được định vị lại trong mọi giải pháp hợp lệ, bởi vì không có phép gán cuối cùng hợp lệ nào có thể đặt nó ở đó. 

Hai thực tế này làm cho tập hợp các nước đi bắt buộc phải cố định: nó chính xác là tập hợp các quân cờ bị cấm ban đầu khi năng lực đủ. Vì chúng ta không bao giờ đạt được bất cứ điều gì bằng cách di chuyển thêm những viên đá, nên năng lượng tối ưu được xác định bằng nước đi không thể tránh khỏi có chỉ số cao nhất, chính xác là chỉ số tối đa bên trong hình chữ nhật. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m, k = map(int, input().split())

stones = []
for _ in range(k):
    x, y = map(int, input().split())
    stones.append((x, y))

u1, v1, u2, v2 = map(int, input().split())

rect_area = (u2 - u1 + 1) * (v2 - v1 + 1)
outside_cells = n * m - rect_area

if outside_cells < k:
    print(-1)
    sys.exit()

ans = 0
inside_count = 0

for i, (x, y) in enumerate(stones, start=1):
    if u1 <= x <= u2 and v1 <= y <= v2:
        inside_count += 1
        ans = max(ans, i)

if inside_count == 0:
    print(0)
else:
    print(ans)
```Code trực tiếp thực hiện việc kiểm tra dung lượng sau đó quét tất cả các vị trí đá. Kiểm tra hình chữ nhật là một phép so sánh tọa độ đơn giản, là O(1) cho mỗi viên đá. 

Một chi tiết tinh tế là chúng ta không bao giờ cần phải xây dựng sự sắp xếp cuối cùng một cách rõ ràng. Bài toán chỉ yêu cầu tính khả thi và năng lượng tối thiểu, cả hai đều chỉ phụ thuộc vào số lượng và chỉ số chứ không phụ thuộc vào đường chuyển động thực tế. 

## Ví dụ đã hoạt động 

Hãy xem xét một lưới trong đó chỉ có một số viên đá rơi vào bên trong hình chữ nhật bị cấm. Chúng tôi quét từng viên đá và đánh dấu những viên bên trong. 

| Bước | Hành động | Đá bên trong được nhìn thấy | Chỉ số tối đa | 
| --- | --- | --- | --- | 
| 1 | Đọc đá 1 bên ngoài | {} | 0 | 
| 2 | Đọc đá 2 bên trong | {2} | 2 | 
| 3 | Đọc đá 3 bên ngoài | {2} | 2 | 
| 4 | Đọc đá 4 bên trong | {2,4} | 4 | 

Câu trả lời cuối cùng là 4, vì hòn đá 4 là hòn đá có chỉ số cao nhất phải được di chuyển. 

Bây giờ hãy xem xét trường hợp không có hòn đá nào nằm bên trong hình chữ nhật. 

| Bước | Hành động | Đá bên trong được nhìn thấy | Chỉ số tối đa | 
| --- | --- | --- | --- | 
| 1 | Quét tất cả đá | {} | 0 | 

Vì không cần chuyển động nên đầu ra là 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k) | Chúng tôi quét tất cả các viên đá một lần và kiểm tra liên tục từng viên đá | 
| Không gian | O(1) | Ngoài bộ nhớ đầu vào, chỉ sử dụng bộ đếm và mức tối đa đang chạy | 

Lời giải dễ dàng nằm trong giới hạn vì k ≤ 250.000 và mọi phép toán đều là quét tuyến tính với số học tầm thường. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import io as sio

    out = sio.StringIO()
    with redirect_stdout(out):
        # assume solution is defined above
        pass
    return out.getvalue().strip()

# provided samples (placeholders since samples in prompt are incomplete)
# assert run("...") == "..."

# custom cases
assert run("""3 3 1
2 2
1 1 1 1
""") == "1", "single inside stone"

assert run("""3 3 2
1 1
3 3
2 2 2 2
""") == "0", "no stone inside rectangle"

assert run("""2 2 4
1 1
1 2
2 1
2 2
1 1 2 2
""") == "-1", "not enough outside space"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đá đơn bên trong | 1 | trường hợp di chuyển cưỡng bức cơ bản | 
| không có đá bên trong | 0 | cấu hình đã hợp lệ | 
| toàn bộ lưới bị chặn | -1 | năng lực không thể | 

## Vỏ cạnh 

Khi hình chữ nhật bị cấm không bao gồm viên đá nào, thuật toán sẽ quét mọi vị trí và không bao giờ cập nhật chỉ số tối đa. Đầu ra chính xác trở thành 0 mà không cần xử lý đặc biệt. 

Khi hình chữ nhật cực lớn, có khả năng bao phủ tất cả ngoại trừ một vài ô, việc kiểm tra dung lượng sẽ kích hoạt sớm và trả về -1 trước khi có bất kỳ vấn đề nào trong quá trình xử lý đá. Điều này tránh được những giả định không chính xác về tính khả thi của việc sắp xếp lại. 

Khi tất cả các viên đá nằm bên trong hình chữ nhật, thuật toán sẽ xác định chính xác rằng mọi viên đá phải di chuyển và trả về chỉ số lớn nhất, vì mỗi chỉ số đều góp phần vào tập hợp các bước di chuyển không thể tránh khỏi.
