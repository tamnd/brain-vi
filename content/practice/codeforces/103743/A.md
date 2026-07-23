---
title: "CF 103743A - PENTA GIẾT!"
description: "Chúng ta được cung cấp nhật ký theo thứ tự thời gian về các sự kiện tiêu diệt trong một trận đấu. Mỗi sự kiện nói rằng một người chơi sẽ loại bỏ một người chơi khác. Mặc dù nạn nhân ngay lập tức hồi sinh và có thể bị giết lần nữa nhưng chúng ta chỉ quan tâm đến trình tự ai giết ai theo thời gian."
date: "2026-07-02T08:58:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103743
codeforces_index: "A"
codeforces_contest_name: "2022 Jiangsu Collegiate Programming Contest"
rating: 0
weight: 103743
solve_time_s: 52
verified: true
draft: false
---

[CF 103743A - PENTA KILL!](https://codeforces.com/problemset/problem/103743/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp nhật ký theo thứ tự thời gian về các sự kiện tiêu diệt trong một trận đấu. Mỗi sự kiện nói rằng một người chơi sẽ loại bỏ một người chơi khác. Mặc dù nạn nhân ngay lập tức hồi sinh và có thể bị giết lần nữa nhưng chúng ta chỉ quan tâm đến trình tự ai giết ai theo thời gian. 

Nhiệm vụ là xác định xem có tồn tại ít nhất một người chơi, tại một thời điểm nào đó trong trận đấu, đã đạt được "penta kill" hay không. Điều này có nghĩa là trong năm lần tiêu diệt liên tiếp được thực hiện bởi cùng một người chơi, các nạn nhân đều khác nhau. Các lần tiêu diệt phải liên tiếp về mặt thời gian giữa các hành động của người chơi đó, không nhất thiết phải liên tiếp các sự kiện toàn cầu liên quan đến tất cả người chơi. 

Đầu ra là một phán đoán duy nhất: liệu trình tự đó có tồn tại đối với bất kỳ người chơi nào hay không. 

Các ràng buộc rất nhỏ, với tối đa 1000 sự kiện tiêu diệt. Điều này ngay lập tức gợi ý rằng mọi lời giải theo thời gian bậc hai đều sẽ an toàn, nhưng cấu trúc của bài toán cho phép quét tuyến tính trực tiếp với công không đổi trên mỗi sự kiện. 

Một điểm tinh tế là giải thích “liên tiếp”. Một cách giải thích ngây thơ có thể xem xét sự liền kề toàn cầu của các sự kiện, nhưng điều đó sẽ kết hợp không chính xác những kẻ giết người khác nhau. Cách giải thích chính xác là tùy theo mỗi người chơi: chúng tôi chỉ xem xét trình tự tiêu diệt được thực hiện bởi một người chơi theo thứ tự thời gian. 

Trường hợp thứ hai là sự lặp lại của nạn nhân. Một người chơi có thể giết năm lần liên tiếp nhưng lặp lại một nạn nhân, điều này làm mất hiệu lực của lần tiêu diệt penta. Ví dụ, trình tự như A giết X, A giết X, A giết Y, A giết Z, A giết W sẽ thất bại vì X xuất hiện hai lần trong số năm lần giết liên tiếp. 

Một trường hợp đặc biệt khác là việc tiêu diệt penta có thể xảy ra ở bất cứ đâu, không nhất thiết phải bắt đầu từ lần tiêu diệt đầu tiên hoặc kết thúc ở lần tiêu diệt cuối cùng. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là, đối với mỗi người chơi, thu thập tất cả số lần giết của họ theo thứ tự và sau đó kiểm tra mọi cửa sổ có kích thước năm, xác minh xem năm nạn nhân có khác biệt hay không. Nếu có k lần tiêu diệt đối với một người chơi, thì chi phí này là O(k * 5) cho mỗi người chơi, điều này thực sự tuyến tính trong số lần tiêu diệt của họ. Trên tất cả người chơi, tổng số này vẫn là O(n) vì mỗi sự kiện được xử lý một số lần không đổi. 

Một cách tiếp cận toàn cầu ngây thơ hơn sẽ cố gắng kiểm tra mọi đoạn liền kề có độ dài năm trong danh sách sự kiện đầy đủ và xác minh xem tất cả các vụ tiêu diệt có phải do cùng một người chơi thực hiện và có nạn nhân riêng biệt hay không. Điều này không thành công về mặt khái niệm vì điều kiện “trong một hàng” không đề cập đến sự liền kề tổng thể và cũng khó khái quát hóa một cách chính xác hơn. 

Quan sát quan trọng là chúng tôi không bao giờ cần lưu trữ nhiều hơn năm lần tiêu diệt cuối cùng cho mỗi người chơi. Khi một người chơi đã thực hiện ít nhất năm lần tiêu diệt, chỉ năm lần tiêu diệt gần đây nhất mới có ý nghĩa quan trọng để phát hiện một lần tiêu diệt penta hợp lệ kết thúc tại thời điểm đó. Điều này chuyển vấn đề thành việc duy trì một cửa sổ trượt cho mỗi người chơi và kiểm tra điều kiện kích thước cố định nhỏ. 

Vì n tối đa là 1000 nên chúng tôi có thể duy trì từ điển một cách an toàn từ tên người chơi đến danh sách (hoặc deque) các nạn nhân gần đây của họ. Mỗi lần tiêu diệt mới sẽ cập nhật cấu trúc đó trong thời gian O(1) và kiểm tra tối đa năm giá trị. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên mỗi cửa sổ người chơi | O(n) | O(n) | Đã chấp nhận | 
| Cửa sổ trượt cho mỗi người chơi | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Đối với mỗi người chơi, chúng tôi duy trì trình tự nạn nhân mà họ đã giết cho đến nay, nhưng chúng tôi chỉ giữ năm mục cuối cùng vì bất kỳ mục nào trước đó không thể đóng góp vào một lần tiêu diệt penta hợp lệ mới kết thúc sau đó.

1. Đọc từng sự kiện tiêu diệt theo thứ tự thời gian. Mỗi sự kiện đưa ra một kẻ giết người và một nạn nhân. Thứ tự này rất quan trọng vì “liên tiếp” phụ thuộc vào thời gian. 
2. Đối với mỗi sự kiện, hãy thêm nạn nhân vào danh sách cá nhân của kẻ giết người về những vụ giết người gần đây. Nếu danh sách vượt quá năm phần tử, hãy loại bỏ phần tử cũ nhất. Điều này đảm bảo chúng tôi chỉ theo dõi hậu tố phù hợp nhất trong lịch sử của họ. 
3. Sau khi cập nhật danh sách, nếu kẻ giết người hiện có ít nhất năm vụ giết người được ghi lại, hãy kiểm tra xem năm nạn nhân cuối cùng có khác biệt hay không. Đây là điều kiện duy nhất cần thiết để xác nhận một lần tiêu diệt penta kết thúc tại sự kiện này. 
4. Nếu bất kỳ người chơi nào thỏa mãn điều kiện khác biệt cho năm lần tiêu diệt cuối cùng của họ, chúng tôi có thể kết luận ngay câu trả lời là tích cực và ngừng xử lý các sự kiện tiếp theo. 
5. Nếu chúng tôi xử lý xong tất cả các sự kiện mà không tìm thấy người chơi như vậy thì câu trả lời là phủ định. 

Lý do chúng tôi chỉ kiểm tra năm mục cuối cùng là vì bất kỳ lần tiêu diệt penta hợp lệ nào đều phải kết thúc tại sự kiện hiện tại đối với người chơi đó và mọi cửa sổ ứng cử viên trước đó sẽ đã được kiểm tra khi nó kết thúc. 

### Tại sao nó hoạt động 

Đối với bất kỳ người chơi nào, mọi lần tiêu diệt penta có thể xảy ra đều tương ứng với một khối liền kề gồm năm lần tiêu diệt của chính họ theo thứ tự thời gian. Bằng cách duy trì một cửa sổ cuộn có kích thước năm, chúng tôi đảm bảo rằng mọi khối như vậy đều được kiểm tra chính xác một lần, tại thời điểm nó trở thành hậu tố trong lịch sử của người chơi. Vì chúng tôi không bao giờ loại bỏ thông tin cần thiết cho các cửa sổ trong tương lai và luôn lưu giữ năm sự kiện gần đây nhất nên không thể bỏ qua chuỗi hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    recent = {}  # player -> list of last kills
    
    for _ in range(n):
        a, b = input().split()
        
        if a not in recent:
            recent[a] = []
        
        recent[a].append(b)
        
        if len(recent[a]) > 5:
            recent[a].pop(0)
        
        if len(recent[a]) == 5:
            if len(set(recent[a])) == 5:
                print("PENTA KILL!")
                return
    
    print("SAD:(")

if __name__ == "__main__":
    solve()
```Cấu trúc cốt lõi là một từ điển được khóa theo tên người chơi, chỉ lưu trữ một số nạn nhân cuối cùng của họ. Điều này tránh mọi nhu cầu lưu trữ toàn bộ lịch sử. 

Chi tiết triển khai tinh tế duy nhất là bước cắt bớt cửa sổ trượt. Chúng tôi luôn xóa khỏi đầu khi danh sách vượt quá năm phần tử, đảm bảo kích thước không đổi. Sử dụng danh sách là đủ vì kích thước tối đa là cố định và nhỏ, do đó các hoạt động khấu hao O(1) là đủ. 

Chúng tôi cũng dựa vào việc xây dựng tập hợp để kiểm tra tính duy nhất. Vì kích thước cửa sổ tối đa là năm nên việc kiểm tra này là thời gian không đổi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một trình tự đơn giản hóa: 

| Bước | Kẻ giết người | Nạn nhân | Trạng thái gần đây (kẻ giết người) | Khác biệt 5 cuối cùng? | 
| --- | --- | --- | --- | --- | 
| 1 | A | x | [x] | Không | 
| 2 | A | y | [x, y] | Không | 
| 3 | A | z | [x, y, z] | Không | 
| 4 | A | w | [x, y, z, w] | Không | 
| 5 | A | v | [x, y, z, w, v] | Có | 

Ở bước 5, cả 5 nạn nhân đều khác nhau nên thuật toán ngay lập tức đưa ra kết quả dương tính. 

Điều này chứng tỏ rằng chúng ta chỉ cần kiểm tra hậu tố kết thúc ở mỗi sự kiện. 

### Ví dụ 2 

| Bước | Kẻ giết người | Nạn nhân | Trạng thái gần đây (A) | Khác biệt 5 cuối cùng? | 
| --- | --- | --- | --- | --- | 
| 1 | A | x | [x] | Không | 
| 2 | A | x | [x, x] | Không | 
| 3 | A | y | [x, x, y] | Không | 
| 4 | A | z | [x, x, y, z] | Không | 
| 5 | A | w | [x, x, y, z, w] | Không | 

Mặc dù A giết được năm mạng liên tiếp nhưng việc lặp lại x sẽ phá vỡ điều kiện. Bản sao được ghi lại ngay lập tức bằng cách kiểm tra kích thước đã đặt. 

Điều này cho thấy tại sao chỉ đếm số lần giết là không đủ nếu không xử lý các nạn nhân riêng biệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi sự kiện cập nhật cấu trúc có kích thước không đổi và thực hiện kiểm tra tính duy nhất theo thời gian liên tục | 
| Không gian | O(n) | Tối đa một danh sách nhỏ cho mỗi người chơi, mỗi danh sách lưu trữ tối đa năm phần tử | 

Các ràng buộc cho phép tối đa 1000 sự kiện, do đó, việc xử lý tuyến tính với chi phí không đổi nhỏ dễ dàng đủ nhanh. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# sample-like case: valid penta kill
assert run("""5
A x
A y
A z
A w
A v
""") == "PENTA KILL!"

# repetition breaks it
assert run("""5
A x
A x
A y
A z
A w
""") == "SAD:("

# mixed killers, only A matters
assert run("""6
B x
A p
A q
B y
A r
A s
""") == "SAD:("

# penta kill not contiguous in global sense but valid per player
assert run("""7
A a
B x
A b
C y
A c
A d
A e
""") == "PENTA KILL!"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều giống nhau sát thủ khác biệt | PENTA GIẾT! | trường hợp thành công cơ bản | 
| nạn nhân lặp đi lặp lại | BUỒN:( | xử lý trùng lặp | 
| người chơi xen kẽ | BUỒN:( | theo dõi mỗi người chơi | 
| giết chết rải rác | PENTA GIẾT! | bỏ qua sự liền kề toàn cầu | 

## Vỏ cạnh 

Trường hợp một cạnh là khi tiêu diệt penta xảy ra chính xác ở cuối nhật ký. Thuật toán vẫn phát hiện ra nó vì mọi sự kiện đều kích hoạt kiểm tra sau khi cập nhật cửa sổ. Ví dụ: nếu năm sự kiện cuối cùng của một người chơi đều khác biệt thì lần lặp cuối cùng sẽ bắt được sự kiện đó ngay lập tức và mang lại kết quả thành công. 

Một trường hợp khác là khi một người chơi có nhiều hơn năm lần tiêu diệt và lần tiêu diệt penta hợp lệ xảy ra muộn hơn trong lịch sử của họ. Vì chúng tôi luôn chỉ giữ lại năm lần tiêu diệt cuối cùng nên các chuỗi cũ hơn sẽ tự động bị loại bỏ, nhưng điều này an toàn vì bất kỳ chuỗi hợp lệ nào cũng phải kết thúc trong năm lần tiêu diệt cuối cùng đó tại thời điểm kết thúc. Cửa sổ trượt đảm bảo chúng ta không bao giờ cần đến lịch sử trước đó. 

Một trường hợp tinh vi cuối cùng là nhiều người chơi có khả năng đạt được penta kill. Thuật toán dừng ở lần phát hiện đầu tiên, điều này đúng vì đầu ra chỉ yêu cầu sự tồn tại chứ không yêu cầu nhận dạng hay đếm.
