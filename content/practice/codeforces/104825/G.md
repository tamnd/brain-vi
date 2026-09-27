---
title: "CF 104825G - Chiến tranh"
description: "Chúng ta được cung cấp một lưới trong đó mỗi ô trống hoặc chứa một đơn vị kẻ thù. Các ô trống và khu vực ngoài lưới đã được chúng tôi kiểm soát. Lưới bắt đầu với tất cả các ô của kẻ thù vẫn còn sống và mục tiêu là loại bỏ mọi ô của kẻ thù."
date: "2026-06-28T12:32:34+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104825
codeforces_index: "G"
codeforces_contest_name: "The 17-th BIT Campus Programming Contest - Onsite Round"
rating: 0
weight: 104825
solve_time_s: 48
verified: true
draft: false
---

[CF 104825G - Chiến tranh](https://codeforces.com/problemset/problem/104825/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một lưới trong đó mỗi ô trống hoặc chứa một đơn vị kẻ thù. Các ô trống và khu vực ngoài lưới đã được chúng tôi kiểm soát. Lưới bắt đầu với tất cả các ô của kẻ thù vẫn còn sống và mục tiêu là loại bỏ mọi ô của kẻ thù. 

Tại bất kỳ thời điểm nào, chúng ta chỉ được phép tấn công một ô địch liền kề với ít nhất một ô thân thiện, trong đó sự thân thiện đến từ ô số 0 trong lưới hoặc đường viền bên ngoài. Khi chúng tôi tấn công một ô như vậy, chúng tôi phải trả một chi phí phụ thuộc vào cấu trúc của cấu hình kẻ thù còn lại tại thời điểm đó: chi phí là một đơn vị cho chính ô đó cộng với một đơn vị bổ sung cho mỗi ô lân cận bốn hướng vẫn chứa kẻ thù tại thời điểm loại bỏ. Sau khi trả chi phí, tế bào đó trở nên thân thiện. 

Nhiệm vụ là chọn thứ tự tiêu diệt toàn bộ ô địch sao cho tổng chi phí là nhỏ nhất. 

Các ràng buộc cho phép các lưới có kích thước lên tới 1000 x 1000, tức là lên tới một triệu ô. Bất kỳ giải pháp nào cố gắng mô phỏng quy trình từng bước trong khi tính toán lại số lượng hàng xóm một cách linh hoạt cho mỗi lần xóa sẽ quá chậm. Ngay cả cách tiếp cận theo thời gian tuyến tính cho mỗi lần loại bỏ cũng sẽ dẫn đến khoảng$10^{12}$trong trường hợp xấu nhất là không thể thực hiện được. 

Một điểm tinh tế là chi phí phụ thuộc vào trạng thái hiện tại của lưới điện chứ không phải trạng thái ban đầu. Điều này khiến người ta dễ nghĩ rằng cần phải có sự sắp xếp hoặc mô phỏng tham lam. Một cạm bẫy khác là giả định rằng việc loại bỏ các ô theo thứ tự BFS hoặc chu vi sẽ làm thay đổi sự đóng góp của các cạnh một cách phức tạp, trong khi trên thực tế, chi phí có tính chất cộng gộp tiềm ẩn. 

Các trường hợp cạnh chủ yếu là về cấu trúc: 

Nếu tất cả các ô của kẻ thù đều bị cô lập, chẳng hạn như một mô hình giống như bàn cờ gồm các ô đơn lẻ, thì mỗi lần loại bỏ sẽ tốn chính xác một ô, vì không có ô nào có ô địch lân cận. Câu trả lời bằng số lượng. 

Nếu tất cả các ô đều là kẻ thù trong một ô được lấp đầy$n \times m$lưới, trực giác ngây thơ có thể cho thấy chi phí phụ thuộc nhiều vào thứ tự loại bỏ, nhưng trên thực tế, cấu trúc buộc mỗi cặp liền kề phải đóng góp chính xác một lần bất kể trình tự. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng rõ ràng tất cả các lệnh loại bỏ hợp lệ. Ở mỗi bước, chúng tôi sẽ xác định tất cả các ô địch hiện có thể tháo rời, thử loại bỏ từng ô và theo dõi chi phí phát sinh theo cách đệ quy hoặc thông qua quay lui. Điều này mô hình hóa chính xác vấn đề vì nó tuân theo định nghĩa quy tắc chính xác, nhưng hệ số phân nhánh lớn và số lượng hoán vị của việc loại bỏ là$(nm)!$, lớn về mặt thiên văn ngay cả đối với các lưới nhỏ. Ngay cả khi chúng ta cắt tỉa bằng cách chỉ xem xét các ô biên giới hợp lệ, không gian trạng thái vẫn theo cấp số nhân. 

Quan sát quan trọng là hàm chi phí có tính chất cục bộ và phân rã trên các ô và các mối quan hệ lân cận. Mỗi ô luôn đóng góp một chi phí cơ bản khi nó bị loại bỏ. Chi phí bổ sung đến từ việc đếm xem có bao nhiêu nước láng giềng vẫn còn là kẻ thù vào thời điểm đó. Thay vì theo dõi sự tiến triển của thời gian, chúng ta có thể diễn giải lại từng điểm lân cận giữa hai ô đối phương như được tích điện chính xác một lần, khi ô sau cùng trong thứ tự loại bỏ được xử lý. 

Điều này biến vấn đề từ một quá trình động thành một vấn đề đếm tĩnh: mỗi ô địch đóng góp một và mỗi cặp ô địch liền kề đóng góp thêm một ô. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | Hàm mũ | O(nm) | Quá chậm | 
| Đếm các ô và vùng lân cận | O(nm) | O(nm) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tránh mô phỏng hoàn toàn việc xóa và thay vào đó tính toán các khoản đóng góp trực tiếp từ lưới ban đầu. 

1. Đầu tiên, quét toàn bộ lưới và đếm xem có bao nhiêu ô chứa kẻ thù. Điều này mang lại sự đóng góp chi phí cơ bản vì mỗi ô địch bị loại bỏ chính xác một lần và luôn trả ít nhất một đơn vị. 
2. Tiếp theo, kiểm tra từng ô của kẻ thù và chỉ kiểm tra các ô bên phải và bên dưới của nó. Đối với mỗi cặp ô địch liền kề, chúng tôi tính thêm một chi phí. Chúng ta chỉ kiểm tra bên phải và bên dưới để tránh tính hai lần, vì mỗi phần kề là vô hướng. 
3. Cộng số cơ số và số kề để có kết quả cuối cùng. 

Lý do chúng tôi chỉ xem xét hướng phải và hướng xuống là vì mọi điểm kề giữa hai ô đối phương đều có chính xác hai điểm cuối và việc kiểm tra cả hai bên sẽ tính cùng một cặp hai lần. 

### Tại sao nó hoạt động 

Hãy coi thứ tự loại bỏ là tùy ý nhưng cố định. Mỗi ô đóng góp chi phí cơ bản của nó đúng một lần. Bây giờ hãy xem xét bất kỳ cặp ô kẻ thù liền kề u và v nào. Một trong số chúng sẽ bị loại bỏ trước tiên. Khi điều đó xảy ra, ô kia vẫn là kẻ thù, vì vậy nó đóng góp thêm đúng một chi phí cho việc loại bỏ ô đầu tiên. Khi ô thứ hai bị xóa, ô đầu tiên đã biến mất và không đóng góp gì. Vì vậy, mỗi cặp liền kề đóng góp tổng cộng chính xác một đơn vị, không phụ thuộc vào thứ tự. Điều này làm cho tổng chi phí bằng số lượng ô địch cộng với số cặp địch liền kề. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())
g = [list(map(int, input().split())) for _ in range(n)]

ones = 0
edges = 0

for i in range(n):
    for j in range(m):
        if g[i][j] == 1:
            ones += 1
            if i + 1 < n and g[i + 1][j] == 1:
                edges += 1
            if j + 1 < m and g[i][j + 1] == 1:
                edges += 1

print(ones + edges)
```Mã trực tiếp thực hiện việc phân tách câu trả lời thành các đóng góp nút và đóng góp cạnh. Các vòng lặp lồng nhau đảm bảo mỗi ô được truy cập một lần. Séc`(i + 1, j)`Và`(i, j + 1)`thực thi việc đếm duy nhất các cặp kề. Các điều kiện biên được xử lý một cách tự nhiên bằng cách kiểm tra chỉ mục, do đó không cần thêm phần đệm hoặc lưới canh gác. 

## Ví dụ đã hoạt động 

Hãy xem xét lưới mẫu:```
0 0 0 0 0
0 0 0 1 0
0 1 1 1 0
0 0 1 0 0
0 0 0 0 0
```Chúng tôi chỉ theo dõi các ô của kẻ thù và các vùng lân cận bên phải/dưới của chúng. 

| Bước | Tế bào | Là kẻ thù | Những cái mới | Các cạnh mới | 
| --- | --- | --- | --- | --- | 
| quét | (2,3) | vâng | +1 | +1 (đến (2,4)) | 
| quét | (3,2) | vâng | +1 | +1 (đến (3,3)) | 
| quét | (3,3) | vâng | +1 | +1 (đến (3,4)) +1 (đến (4,3)) | 
| quét | (3,4) | vâng | +1 | 0 | 
| quét | (4,3) | vâng | +1 | 0 | 

Điều này mang lại tổng cộng 5 ô địch và 4 cặp lân cận, dẫn đến 9. 

Dấu vết này cho thấy câu trả lời hoàn toàn được xác định bởi cấu trúc tĩnh. “Lệnh tấn công” động không bao giờ xuất hiện, tuy nhiên kết quả đã khớp với bất kỳ chuỗi tối ưu hợp lệ nào. 

Bây giờ hãy xem xét một cấu hình hoàn toàn biệt lập:```
1 0
0 1
```| Tế bào | Những cái | Cạnh | 
| --- | --- | --- | 
| (1,1) | +1 | 0 | 
| (2,2) | +1 | 0 | 

Tổng chi phí là 2, xác nhận rằng các ô bị cô lập chỉ đóng góp chi phí cơ bản. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nm) | Mỗi ô được truy cập một lần và được kiểm tra theo thời gian không đổi | 
| Không gian | O(1) | Chỉ các bộ đếm được sử dụng ngoài bộ nhớ đầu vào | 

Giải pháp này phù hợp một cách thoải mái trong giới hạn bởi vì một lần vượt qua một$10^6$lưới chỉ thực hiện công việc không đổi trên mỗi ô, nằm trong giới hạn 1 giây thông thường trong Python khi được triển khai với I/O nhanh. 

## Trường hợp thử nghiệm```python
import sys, io

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, m = map(int, input().split())
    g = [list(map(int, input().split())) for _ in range(n)]

    ones = 0
    edges = 0

    for i in range(n):
        for j in range(m):
            if g[i][j] == 1:
                ones += 1
                if i + 1 < n and g[i + 1][j] == 1:
                    edges += 1
                if j + 1 < m and g[i][j + 1] == 1:
                    edges += 1

    return str(ones + edges)

def run(inp: str) -> str:
    return solve(inp)

# provided sample
assert run("""5 5
0 0 0 0 0
0 0 0 1 0
0 1 1 1 0
0 0 1 0 0
0 0 0 0 0
""") == "9"

# single cell
assert run("""1 1
1
""") == "1"

# no enemies
assert run("""2 3
0 0 0
0 0 0
""") == "0"

# all enemies 2x2
assert run("""2 2
1 1
1 1
""") == "8"

# checkerboard
assert run("""3 3
1 0 1
0 1 0
1 0 1
""") == "5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1x1 đơn | 1 | trường hợp cơ sở tối thiểu | 
| lưới trống | 0 | không xử lý đóng góp | 
| đầy đủ 2x2 | 8 | đếm lân cận dày đặc | 
| bàn cờ | 5 | thành phần biệt lập và không tính hai lần | 

## Vỏ cạnh 

Một lưới hoàn toàn trống được xử lý một cách tự nhiên vì quá trình quét không bao giờ tăng bộ đếm, tạo ra số 0 mà không có logic đặc biệt. 

Một ô địch cũng hoạt động chính xác vì nó đóng góp một ô vào số lượng cơ sở và không có ô lân cận nào để tạo ra các đóng góp lân cận. Thuật toán không cố gắng truy cập các chỉ mục không hợp lệ vì tất cả các kiểm tra lân cận đều được bảo vệ. 

Lưới được lấp đầy là nơi mà độ chính xác không rõ ràng nhất. Đối với một$n \times m$khối, mỗi cạnh kề bên trong được tính chính xác một lần thông qua quét phải và quét xuống, phù hợp với ý tưởng rằng mỗi cạnh đóng góp một đơn vị chi phí bất kể thứ tự loại bỏ.
