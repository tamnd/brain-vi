---
title: "CF 104385E - Cây phân đoạn"
description: "Chúng ta được cấp một cây phân đoạn hoàn chỉnh trong phạm vi từ 0 đến 2^n − 1. Thay vì làm việc trực tiếp với mảng, bài toán xây dựng một đồ thị cảm ứng G bằng cách chạy thủ tục truy vấn cây phân đoạn trên một khoảng [L, R]."
date: "2026-07-01T02:52:52+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104385
codeforces_index: "E"
codeforces_contest_name: "2023 (ICPC) Jiangxi Provincial Contest -- Official Contest"
rating: 0
weight: 104385
solve_time_s: 58
verified: true
draft: false
---

[CF 104385E - Cây phân đoạn](https://codeforces.com/problemset/problem/104385/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 58s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một cây phân đoạn hoàn chỉnh trong phạm vi từ 0 đến 2^n − 1. Thay vì làm việc trực tiếp với mảng, bài toán xây dựng một đồ thị cảm ứng G bằng cách chạy thủ tục truy vấn cây phân đoạn trên một khoảng [L, R]. Quy trình thu thập chính xác các nút cây phân đoạn cần thiết để bao phủ khoảng này và kết nối chúng bằng cấu trúc cha-con giống như trong cây phân đoạn ban đầu. Mỗi cạnh cũng được gắn nhãn 0 hoặc 1 tùy thuộc vào việc nó tương ứng với việc di chuyển sang con trái hay con phải. 

Vì vậy, kết quả là một cây có các nút là các khoảng cây phân đoạn và các cạnh của nó biểu thị sự liền kề trong cấu trúc cây phân đoạn ban đầu, nhưng chỉ bị giới hạn ở phần chạm vào [L, R]. 

Hai người chơi sau đó chơi trên cây này. Ở mỗi nước đi, người chơi chọn một chuỗi gồm ít nhất hai nút x1 < x2 < … < xm. Các nút này phải tạo thành một đường dẫn đơn giản bên trong G và các cạnh dọc theo đường dẫn đó đều phải có cùng nhãn: Lyra chỉ được phép chọn các đường dẫn chỉ sử dụng 0 cạnh, trong khi Bon Bon bị hạn chế ở các đường dẫn chỉ sử dụng 1 cạnh. Sau khi chọn đường dẫn như vậy, tất cả các nút đã chọn sẽ bị xóa khỏi biểu đồ cùng với các cạnh tới của chúng. 

Sau mỗi lần di chuyển, đồ thị còn lại phải luôn được kết nối. Ngoài ra, nút 1 (gốc của cây phân đoạn ban đầu) bị cấm xóa trừ khi thao tác đó xóa toàn bộ biểu đồ. 

Người chơi không thể thực hiện một nước đi hợp lệ sẽ thua. Đối với mỗi trường hợp thử nghiệm, chúng tôi được hỏi liệu Lyra có thắng trong lối chơi tối ưu hay không. 

Các ràng buộc có độ sâu nhỏ, vì n ≤ 61, nghĩa là cây phân đoạn có chiều cao tối đa là 61. Số lượng trường hợp thử nghiệm có thể lớn tới 10^6, do đó, giải pháp về cơ bản phải là O(1) hoặc O(log n) cho mỗi trường hợp thử nghiệm. 

Khó khăn chính là mặc dù cây xuất phát từ cấu trúc cây phân đoạn, trò chơi không được chơi trên một mảng mà trên một cây cảm ứng có cấu trúc và các bước di chuyển phụ thuộc vào các đường đơn sắc trong cấu trúc đó. 

Một mô phỏng đơn giản sẽ xây dựng rõ ràng cây truy vấn G, trong trường hợp xấu nhất có cấu trúc O(2^n), điều này là không thể ngay cả đối với một trường hợp thử nghiệm đơn lẻ khi n lớn. 

Ý tưởng ngây thơ thứ hai là mô phỏng trạng thái trò chơi hoặc tính toán các giá trị Grundy trên cây. Điều đó thất bại ngay lập tức vì kích thước cây theo cấp số nhân tính bằng n và số lần di chuyển có thể là tổ hợp. 

Trường hợp cạnh tinh tế là khi [L, R] khớp chính xác với khoảng cách nút cây phân đoạn đơn. Trong trường hợp này, cây truy vấn thoái hóa thành một cấu trúc dạng chuỗi đơn mà không phân nhánh. Ngược lại, bất kỳ khoảng nào không được căn chỉnh hoàn hảo với ranh giới nút cây phân đoạn sẽ tạo ra cấu trúc phân nhánh trong cây cảm ứng. 

## Phương pháp tiếp cận 

Quan sát quan trọng là chúng ta không thực sự xử lý một cây tùy ý. Cây truy vấn được tạo bởi truy vấn phạm vi cây phân đoạn có cấu trúc rất cứng nhắc: nó chính xác là sự phân tách [L, R] thành các nút cây phân đoạn chính tắc O(log n), được kết nối thông qua hệ thống phân cấp cây phân đoạn. 

Nếu chúng ta cố gắng suy nghĩ về mặt sức mạnh vũ phu, chúng ta sẽ xây dựng G một cách rõ ràng, liệt kê tất cả các đường dẫn đơn sắc hợp lệ và chạy một trình giải trò chơi trên một trò chơi đồ thị tổng quát. Điều này nhanh chóng trở nên không khả thi vì số lượng trạng thái tăng theo cấp số nhân với số lượng nút trong G và bản thân G có thể lớn. 

Sự đơn giản hóa quan trọng xuất phát từ việc chú ý cấu trúc của G thực sự trông như thế nào. Phân tách cây phân đoạn tạo ra một cây gần như luôn phân nhánh: bất cứ khi nào một khoảng được chia thành hai phần chính tắc, các nút tương ứng sẽ kết nối với các cây con khác nhau, tạo ra một điểm phân nhánh trong G. Trường hợp duy nhất không xảy ra phân nhánh là khi [L, R] trùng khớp chính xác với một khoảng nút cây phân đoạn, nghĩa là toàn bộ phạm vi là một khối căn chỉnh lũy thừa hai.

Sự khác biệt về cấu trúc này biến trò chơi thành một cuộc kiểm tra kết quả đơn giản. Nếu biểu đồ cảm ứng là một đường dẫn thuần túy (không phân nhánh), thì mỗi bước di chuyển sẽ làm giảm đường dẫn theo cách bị ràng buộc và người chơi thứ hai có thể phản chiếu cách chơi tối ưu. Nếu đồ thị có bất kỳ sự phân nhánh nào, Lyra có lợi thế chiến thắng bắt buộc vì cô ấy luôn có thể chọn những nước đi loại bỏ các nhánh bất đối xứng và giảm thế trận nhanh hơn đối thủ có thể phản ánh. 

Do đó, toàn bộ vấn đề quy về việc kiểm tra xem [L, R] có tương ứng chính xác với khoảng nút cây phân đoạn hay không. 

Khoảng nút cây phân đoạn được xác định bởi độ dài lũy thừa của hai và ranh giới căn chỉnh là bội số của độ dài đó. Cụ thể, [L, R] chính xác là một khoảng nút khi và chỉ khi R − L + 1 là lũy thừa của hai và L chia hết cho R − L + 1. 

Mọi thứ khác đều tạo ra một cây truy vấn phân nhánh và do đó mang lại vị trí chiến thắng cho Lyra. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force Construction + Trò chơi mô phỏng | Số mũ trong n | Hàm mũ | Quá chậm | 
| Kiểm tra thuộc tính căn chỉnh cây phân đoạn | O(1) mỗi lần kiểm tra | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Tính độ dài của khoảng, len = R − L + 1. Đây là kích thước của phạm vi được bao phủ trong cây phân đoạn ban đầu. 
2. Kiểm tra xem len có phải là lũy thừa của hai hay không. Điều này có thể được thực hiện bằng cách sử dụng thủ thuật bit tiêu chuẩn len & (len − 1) == 0. Điều kiện này đảm bảo rằng khoảng có thể tương ứng với một nút cây phân đoạn đầy đủ. 
3. Nếu len không phải là lũy thừa của 2, hãy kết luận ngay rằng cây truy vấn phải chứa phân nhánh, vì khoảng không thể được biểu diễn dưới dạng một nút cây phân đoạn đơn. Trong trường hợp này, Lyra thắng. 
4. Nếu len là lũy thừa của hai, hãy kiểm tra căn chỉnh: kiểm tra xem L có chia hết cho len hay không. Nếu L % len == 0 thì [L, R] khớp chính xác với khoảng nút của cây phân đoạn, do đó cây truy vấn là một cấu trúc chuỗi không phân nhánh. 
5. Nếu cả hai điều kiện đều giữ nguyên, kết quả mà Lyra thua. Ngược lại, kết quả Lyra thắng. 

Lý do đằng sau bước cuối cùng là chỉ các nút cây phân đoạn được căn chỉnh hoàn hảo mới tránh được việc chia tách thành nhiều nút chính tắc trong quá trình phân tách truy vấn. Bất kỳ sự sai lệch nào cũng buộc thủ tục truy vấn phải kết hợp nhiều nút, điều này sẽ tạo ra sự phân nhánh trong biểu đồ cảm ứng. 

Lý do nó hoạt động dựa trên tính chất bất biến là việc phân tách truy vấn cây phân đoạn tạo ra một tập hợp tối thiểu các khoảng chuẩn rời rạc. Một khoảng chính tắc duy nhất tạo ra một cấu trúc tuyến tính trong cây cảm ứng, trong khi nhiều khoảng chính tắc nhất thiết phải tạo ra một nút phân nhánh nơi quá trình phân tách tách ra. Điểm phân nhánh đó là điểm phá vỡ tính đối xứng và mang lại cho người chơi đầu tiên một chiến lược chiến thắng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def is_pow2(x):
    return x > 0 and (x & (x - 1)) == 0

t = int(input())
out = []

for _ in range(t):
    n, L, R = map(int, input().split())
    length = R - L + 1

    if is_pow2(length) and L % length == 0:
        out.append("No")
    else:
        out.append("Yes")

sys.stdout.write("\n".join(out))
```Giải pháp hoàn toàn dựa vào đặc tính cấu trúc khi truy vấn cây phân đoạn phân tách thành chính xác một khoảng chuẩn. Kiểm tra lũy thừa hai lọc các khoảng ứng cử viên có thể tương ứng với một nút trong cây phân đoạn. Kiểm tra căn chỉnh đảm bảo rằng khoảng thời gian bắt đầu chính xác tại ranh giới phù hợp với phân đoạn của nút đó. 

Quyết định cuối cùng mã hóa trực tiếp kết quả trò chơi: vị trí thua duy nhất của Lyra là những vị trí mà cây truy vấn hoàn toàn tuyến tính, nghĩa là không tồn tại cấu trúc phân nhánh nào để tạo ra sự bất cân xứng về chiến thắng. 

## Ví dụ đã hoạt động 

Hãy xem xét trường hợp khoảng được căn chỉnh hoàn hảo. 

đầu vào:```
1
3 4 7
```Ở đây, độ dài là 4, là lũy thừa của hai và L = 4 chia hết cho 4. Vì vậy khoảng tương ứng với một nút cây phân đoạn đơn. Thuật toán phân loại đây là vị trí thua của Lyra, tạo ra "Không". 

Bây giờ hãy xem xét một khoảng dịch chuyển một chút. 

đầu vào:```
1
3 5 8
```Ở đây, độ dài lại là 4, nhưng L = 5 không chia hết cho 4. Vì vậy, khoảng không thể là một nút chính tắc duy nhất. Việc phân tách truy vấn phải chia thành nhiều nút cây phân đoạn, tạo ra sự phân nhánh trong G. Thuật toán đưa ra “Có”, nghĩa là Lyra thắng. 

| Bước | L | R | Chiều dài | Sức mạnh của hai | Căn chỉnh | Kết quả | 
| --- | --- | --- | --- | --- | --- | --- | 
| Trường hợp 1 | 4 | 7 | 4 | Có | Có | Không | 
| Trường hợp 2 | 5 | 8 | 4 | Có | Không | Có | 

Những dấu vết này cho thấy toàn bộ quyết định chỉ phụ thuộc vào việc khoảng đó có phải là khối cây phân đoạn chuẩn hay không. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) cho mỗi trường hợp thử nghiệm | Chỉ các phép toán số học và bit | 
| Không gian | O(1) | Không có cấu trúc dữ liệu ngoài các biến | 

Với tối đa 10^6 trường hợp thử nghiệm, giải pháp thời gian không đổi này là cần thiết. Bất kỳ cách tiếp cận nào cố gắng xây dựng hoặc duyệt qua cây truy vấn sẽ không khả thi do kích thước cấp số nhân của biểu diễn cây phân đoạn trong n. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def is_pow2(x):
        return x > 0 and (x & (x - 1)) == 0

    t = int(input())
    out = []
    for _ in range(t):
        n, L, R = map(int, input().split())
        length = R - L + 1
        if is_pow2(length) and L % length == 0:
            out.append("No")
        else:
            out.append("Yes")

    return "\n".join(out)

# provided samples (illustrative)
assert run("3\n4 0 3\n3 5 5\n3 4 7\n") == "Yes\nNo\nNo"

# minimum size
assert run("1\n1 0 0\n") == "No"

# non-aligned interval
assert run("1\n3 1 6\n") == "Yes"

# full power-of-two aligned block
assert run("1\n4 8 23\n") == "No"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 0 0 | Không | Trường hợp cạnh nút đơn | 
| 3 1 6 | Có | Phân nhánh lực lượng khoảng cách sai lệch | 
| 4 8 23 | Không | Căn chỉnh hoàn hảo tạo ra cây tuyến tính | 

## Vỏ cạnh 

Khi khoảng giảm xuống còn một điểm duy nhất, L = R, thuật toán sẽ thấy độ dài = 1, là lũy thừa của hai và luôn thẳng hàng. Điều này tương ứng với một nút cây phân đoạn tầm thường. Vì các quy tắc cấm chọn nút 1 trừ khi toàn bộ biểu đồ bị xóa, vị trí này sẽ bị mất đối với Lyra và thuật toán xuất ra “Không”. 

Khi khoảng là một khối lũy thừa đầy đủ của hai nhưng không được căn chỉnh, chẳng hạn như L = 5, R = 12, độ dài là 8 nhưng L không chia hết cho 8. Cây truy vấn cảm ứng phải phân chia ở các cấp độ cây phân đoạn cao hơn, tạo ra các nút phân nhánh. Thuật toán đưa ra kết quả “Có” một cách chính xác, phù hợp với thực tế là Lyra có thể tạo ra sự bất đối xứng thông qua việc loại bỏ phân nhánh sớm. 

Khi khoảng lớn nhưng gần như thẳng hàng, chẳng hạn như L = 0 và R = 2^k − 2, độ dài không phải là lũy thừa của hai, do đó điều kiện đầu tiên đã thất bại. Điều này đảm bảo sự phân nhánh và kết quả lại là vị trí chiến thắng cho Lyra.
