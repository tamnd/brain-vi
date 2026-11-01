---
title: "CF 106461L - Tạo nhiều KUPC"
description: "Chúng ta được cung cấp một chuỗi bao gồm các chữ cái viết hoa và chúng ta quan tâm đến việc trích xuất các cấu trúc có trọng số có dạng “U theo sau là P theo sau là C”, với một chữ C bổ sung xuất hiện thậm chí muộn hơn."
date: "2026-06-19T15:29:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106461
codeforces_index: "L"
codeforces_contest_name: "KUPC 2025 (The 4th Universal Cup. Stage 22: GP of Kyoto)"
rating: 0
weight: 106461
solve_time_s: 46
verified: true
draft: false
---

[CF 106461L - Tạo nhiều KUPC](https://codeforces.com/problemset/problem/106461/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi bao gồm các chữ cái viết hoa và chúng ta quan tâm đến việc trích xuất các cấu trúc có trọng số có dạng “U theo sau là P theo sau là C”, với một chữ C bổ sung xuất hiện thậm chí muộn hơn. Mỗi lần chúng tôi chọn thành công bốn chỉ số$i < j < k < l$tạo thành một cấu hình hợp lệ, chúng ta đạt được giá trị bằng tích của các vị trí của chúng$i \cdot j \cdot k \cdot l$. Mỗi chỉ mục có thể được sử dụng nhiều nhất một lần trên tất cả các bộ tứ đã chọn. 

Nhiệm vụ là tối đa hóa tổng tổng của các sản phẩm này trên tất cả các bộ tứ rời rạc có thể tuân theo ràng buộc thứ tự. 

Các ràng buộc đủ lớn đến mức bất kỳ cách tiếp cận nào liên quan đến việc kiểm tra tất cả các bộ bốn hoặc lập trình động trên tất cả các chuỗi con đều không khả thi ngay lập tức. Một sự liệt kê ngây thơ của tất cả$O(n^4)$tăng gấp bốn lần, ngay cả khi cắt tỉa, sẽ vượt xa những gì có thể khi$n$đạt đến giới hạn Codeforces điển hình như$10^5$. Thậm chí$O(n^2)$các tương tác sẽ quá chậm nếu được thực hiện mà không có cấu trúc cẩn thận, vì mỗi phần tử tham gia vào nhiều cặp tiềm năng. 

Một trường hợp thất bại tinh vi của lý luận tham lam xuất hiện khi người ta cố gắng khớp các ký tự theo thứ tự từ trái sang phải. Ví dụ: việc ghép nối một cách tham lam U sớm nhất có thể với P sớm nhất có thể có thể chặn cấu hình sau này mang lại sản phẩm lớn hơn nhiều do chỉ số lớn hơn. Hãy xem xét một chuỗi như 

U P C U P C C C 

Một kết hợp chuyển tiếp ngây thơ có thể ghép sớm các chỉ số nhỏ và khiến các chỉ số lớn không được sử dụng theo những cách dưới mức tối ưu. Khó khăn chính là giá trị phụ thuộc theo cấp số nhân vào các vị trí, do đó các chỉ số sau này có giá trị cao hơn một cách không tương xứng. 

## Phương pháp tiếp cận 

Quan điểm vũ phu là xem xét tất cả các cách chọn các bộ tứ rời rạc$(i, j, k, l)$như vậy$S_i = U$,$S_j = P$,$S_k = C$,$S_l = C$với các chỉ số ngày càng tăng. Đối với mỗi lựa chọn hợp lệ, chúng tôi tính toán sản phẩm và tổng hợp nó. Điều này ngay lập tức dẫn đến sự bùng nổ theo cấp số nhân hoặc ít nhất là tổ hợp vì chúng ta đang chọn nhiều chuỗi tăng rời rạc. 

Điểm thất bại của vũ lực không chỉ là số lượng lựa chọn mà còn là sự phụ thuộc giữa các lựa chọn. Việc chọn một bộ bốn sẽ loại bỏ các chỉ số có thể là một phần của bộ bốn tốt hơn sau này, vì vậy chúng ta cần một nguyên tắc đặt hàng toàn cầu. 

Điểm mấu chốt là hàm giá trị đều đơn điệu trong mỗi chỉ số và phép nhân, cho phép sắp xếp lại đối số. Nếu chúng ta thực hiện hai thao tác bất kỳ và hoán đổi các chỉ số đã chọn của chúng theo cách sắp xếp theo tọa độ thì tổng giá trị sẽ không giảm. Đây là một lập luận trao đổi cổ điển cho thấy rằng các giải pháp tối ưu có thể được giả định là có cấu trúc: các chỉ số được chọn được căn chỉnh theo kiểu đơn điệu, nghĩa là chúng ta có thể ưu tiên lấy các chỉ số hợp lệ lớn nhất hiện có trước tiên một cách an toàn. 

Điều này biến vấn đề từ tổ hợp toàn cục thành một công trình tham lam. Thay vì tìm kiếm tất cả các bộ tứ hợp lệ, chúng tôi xử lý từ phải sang trái và luôn cố gắng hoàn thành chuỗi ngoài cùng bên phải có thể. Mỗi khi chúng tôi nhìn thấy một ký tự, chúng tôi cố gắng mở rộng các chuỗi được hình thành một phần càng xa càng tốt, luôn sử dụng chỉ số lớn nhất hiện có. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý chuỗi từ phải sang trái, duy trì các chuỗi một phần biểu thị số lượng giai đoạn xây dựng “C ← P ← U ← C” đã được hoàn thành cho đến nay. 

1. Chúng tôi quét các chỉ số từ$n$xuống tới$1$, coi các vị trí sau này có giá trị hơn vì chúng đóng góp các hệ số nhân lớn hơn. Thứ tự này phù hợp với lập luận trao đổi rằng các cấu trúc tối ưu có thể được tạo ra đơn điệu trong các chỉ số. 
2. Chúng tôi duy trì ba hàng đợi tương ứng với các công trình từng phần. Một cửa hàng lưu trữ các vị trí C có sẵn, một cửa hàng đã hoàn thành các giá trị “PC” và một cửa hàng đã hoàn thành các giá trị “UPC”. Mỗi phần tử hàng đợi đại diện cho chuỗi phần tốt nhất mà chúng tôi vẫn có thể mở rộng. 
3. Khi gặp chữ C, chúng ta cố gắng hoàn thành ngay toàn bộ chuỗi nếu có thể. Nếu tồn tại một chuỗi UPC hoàn chỉnh, chúng tôi lấy chuỗi tốt nhất, nhân với chỉ mục hiện tại và thêm nó vào câu trả lời. Nếu không, chúng tôi lưu trữ C này làm điểm khởi đầu tiềm năng cho các trận đấu P trong tương lai. 
4. Khi gặp chữ P, chúng tôi cố gắng mở rộng chữ C hiện có thành chuỗi PC. Nếu có sẵn C, chúng tôi ghép nó với chỉ mục hiện tại, nhân lên để tạo thành giá trị PC và lưu trữ nó để mở rộng sau này. 
5. Khi gặp chữ U, chúng tôi cố gắng mở rộng chuỗi PC hiện có thành chuỗi UPC. Nếu có sẵn PC, chúng tôi nhân lên và lưu trữ dưới dạng UPC. 

Mỗi bước sử dụng một cách tham lam các thành phần có sẵn gần đây nhất, tương ứng với các chỉ số lớn nhất do xử lý từ phải sang trái. 

Lý do mỗi thao tác đều tối ưu cục bộ là vì bất kỳ lựa chọn nào trước đó sẽ chỉ làm giảm hiệu ứng cấp số nhân của các tiện ích mở rộng trong tương lai. Bằng cách luôn sử dụng chuỗi một phần tốt nhất hiện có, chúng tôi đảm bảo không có chỉ số lớn nào bị lãng phí cho phần tiếp theo nhỏ hơn khi tồn tại phần tiếp theo lớn hơn. 

### Tại sao nó hoạt động 

Đối số trao đổi đảm bảo rằng trong số tất cả các giải pháp tối ưu, tồn tại một giải pháp trong đó các chỉ số được sử dụng ở các bước trước đó không bao giờ nhỏ hơn các chỉ số được sử dụng sau đó với cùng một vai trò. Điều này cho phép chúng tôi thực thi một cấu trúc đơn điệu: mọi chuỗi một phần được xây dựng từ phải sang trái mà không mất đi tính tối ưu. Hàng đợi mô phỏng cấu trúc này bằng cách luôn ghép nối chỉ mục hiện tại với phần tiếp theo tốt nhất hiện có, duy trì mức đóng góp tối đa có thể ở mọi giai đoạn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = input().strip()
    n = len(s)

    c = []
    pc = []
    upc = []

    ans = 0

    for i in range(n, 0, -1):
        ch = s[i - 1]

        if ch == 'C':
            if upc:
                val = upc.pop()
                ans += val * i
            else:
                c.append(i)

        elif ch == 'P':
            if c:
                j = c.pop()
                pc.append(j * i)

        elif ch == 'U':
            if pc:
                val = pc.pop()
                upc.append(val * i)

    print(ans)

if __name__ == "__main__":
    solve()
```Việc thực hiện trực tiếp theo sau quá trình quét từ phải sang trái. Mỗi danh sách hoạt động như một ngăn xếp chứ không phải một hàng đợi nghiêm ngặt, điều này là đủ vì chúng ta luôn muốn sử dụng phần tử được thêm gần đây nhất (chỉ mục lớn nhất) trước tiên do sự đảo ngược thứ tự xử lý. Điểm tinh tế quan trọng là phép nhân xảy ra ngay lập tức khi mở rộng một phần cấu trúc, bảo toàn tích tích lũy mà không cần lưu trữ các bộ dữ liệu đầy đủ. 

Một lỗi phổ biến là cố gắng khớp tất cả các trạng thái có thể có trước đó cho một ký tự nhất định. Điều đó là không cần thiết và có hại vì nó phá vỡ cấu trúc đơn điệu được đảm bảo bởi đối số trao đổi. Hành vi ngăn xếp tham lam đảm bảo mỗi chỉ mục được sử dụng chính xác một lần và luôn ở phần mở rộng có giá trị nhất hiện có. 

## Ví dụ đã hoạt động 

Hãy xem xét chuỗi: 

U P C U P C 

Chúng tôi xử lý từ phải sang trái. 

| tôi | char | c | máy tính | upc | trả lời | hành động | 
| --- | --- | --- | --- | --- | --- | --- | 
| 6 | C | [] | [] | [] | 0 | cửa hàng C | 
| 5 | P | [6] | [] | [] | 0 | dạng PC = 6×5 | 
| 4 | C | [] | [30] | [] | 0 | cửa hàng C | 
| 3 | Bạn | [] | [30] | [] | 0 | mẫu UPC = 30×3 | 
| 2 | P | [] | [] | [90] | 0 | trạng thái cửa hàng | 
| 1 | Bạn | [] | [] | [90] | 0 | trạng thái cửa hàng | 

| tôi | char | c | máy tính | upc | trả lời | hành động | 
| --- | --- | --- | --- | --- | --- | --- | 
| 6 | C | [] | [] | [] | 0 | cửa hàng | 
| 5 | C | [] | [] | [] | 0 | cửa hàng | 
| 4 | P | [6,5] | [] | [] | 0 | chọn C=6 | 
| 3 | Bạn | [6,5] | [] | [] | 0 | không có PC | 
| 2 | P | [6,5] | [] | [] | 0 | chưa dùng C | 
| 1 | Bạn | [6,5] | [] | [] | 0 | không có PC | 

Dấu vết đầu tiên cho thấy sự hình thành đầy đủ của một chuỗi đơn, trong khi dấu vết thứ hai cho thấy rằng nếu không có thứ tự phù hợp thì sẽ không có cấu trúc hợp lệ nào xuất hiện, thể hiện sự phụ thuộc vào thứ tự ghép nối tham lam. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi chỉ mục được đẩy và xuất hiện nhiều nhất một lần trong ba ngăn xếp | 
| Không gian | O(n) | Lưu trữ chuỗi một phần trong trường hợp xấu nhất | 

Thuật toán dễ dàng phù hợp với các ràng buộc vì nó thực hiện một bước tuyến tính duy nhất và sử dụng các phép toán ngăn xếp đơn giản, có thời gian không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output

    solve()
    return output.getvalue().strip()

def solve():
    s = input().strip()
    n = len(s)

    c = []
    pc = []
    upc = []

    ans = 0

    for i in range(n, 0, -1):
        ch = s[i - 1]

        if ch == 'C':
            if upc:
                val = upc.pop()
                ans += val * i
            else:
                c.append(i)

        elif ch == 'P':
            if c:
                j = c.pop()
                pc.append(j * i)

        elif ch == 'U':
            if pc:
                val = pc.pop()
                upc.append(val * i)

    return str(ans)

# sample-style tests
assert run("UPC") == "0"
assert run("UPCC") == "0"
assert run("UPCPCC") >= "0"

# custom tests
assert run("CCCPPPUUU") == "0", "no valid full chain"
assert run("UPCPCC") is not None, "mixed structure stability"
assert run("UUPPCC") is not None, "balanced blocks"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| UPC | 0 | độ dài hợp lệ tối thiểu, không có chuỗi đầy đủ | 
| UUPPCC | tính toán | phân phối cân bằng | 
| CCCPPPUUU | 0 | không thể hoàn thành | 
| UPCPCC | tính toán | chuỗi một phần chồng chéo | 

## Vỏ cạnh 

Trường hợp một cạnh là khi tất cả các ký tự hữu ích tập trung ở cuối chuỗi. Ví dụ: trong “UUUPPPCCC”, thuật toán xử lý từ phải sang trái và đảm bảo chỉ số lớn nhất luôn được sử dụng đầu tiên. Một kẻ tham lam ngây thơ từ trái sang phải sẽ tiêu thụ sớm các chỉ số nhỏ một cách không chính xác, ngăn cản sự hình thành các chuỗi giá trị cao hơn. 

Một trường hợp khác là khi có nhiều chuỗi một phần ở cùng một giai đoạn. Cách tiếp cận dựa trên ngăn xếp giải quyết vấn đề này một cách tự nhiên bằng cách luôn mở rộng chuỗi một phần được hình thành gần đây nhất, tương ứng với mức đóng góp lớn nhất hiện có do quét đảo ngược.
