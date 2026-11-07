---
title: "CF 1373B - 01 Trận"
description: "Chúng ta được cấp một chuỗi nhị phân và hai người chơi lần lượt loại bỏ chính xác hai ký tự liền kề khỏi chuỗi đó. Mỗi lần di chuyển sẽ xóa một cặp liền kề, do đó chuỗi sẽ co lại 2 ký tự mỗi lượt."
date: "2026-06-16T12:46:38+07:00"
tags: ["codeforces", "competitive-programming", "games"]
categories: ["algorithms"]
codeforces_contest: 1373
codeforces_index: "B"
codeforces_contest_name: "Educational Codeforces Round 90 (Rated for Div. 2)"
rating: 900
weight: 1373
solve_time_s: 144
verified: true
draft: false
---

[CF 1373B - 01 Trò chơi](https://codeforces.com/problemset/problem/1373/B) 

**Xếp hạng:** 900 
**Thẻ:** trò chơi 
**Thời gian giải:** 2m 24s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi nhị phân và hai người chơi lần lượt loại bỏ chính xác hai ký tự liền kề khỏi chuỗi đó. Mỗi lần di chuyển sẽ xóa một cặp liền kề, do đó chuỗi sẽ co lại 2 ký tự mỗi lượt. Người chơi sẽ thua ngay lập tức khi gặp một chuỗi không tồn tại cặp liền kề, điều này xảy ra khi độ dài chuỗi là 0 hoặc 1. 

Câu hỏi quan trọng không phải là xây dựng nước đi mà là quyết định người chiến thắng nếu cả hai người chơi đều lựa chọn tối ưu. Alice luôn đi trước, vì vậy trò chơi hoàn toàn được xác định bằng tổng số nước đi hợp lệ có thể được thực hiện trước khi chuỗi trở nên quá ngắn để tiếp tục. 

Ràng buộc về độ dài chuỗi là nhỏ, nhiều nhất là 100 và có tới 1000 trường hợp thử nghiệm. Điều này có nghĩa là ngay cả một mô phỏng O(n²) cho mỗi trường hợp thử nghiệm cũng có thể được chấp nhận, nhưng cấu trúc của vấn đề cho thấy chúng ta nên nhắm đến lý do O(n) cho mỗi chuỗi. 

Một điểm tinh tế là việc sắp xếp chính xác các số 0 và 1 không thực sự quan trọng đối với việc chơi lâu dài ngoại trừ việc nước đi có tồn tại hay không. Mỗi lần di chuyển chỉ giảm độ dài đi 2 và không có bước di chuyển nào tạo ra các ràng buộc kề mới ngoài việc thu hẹp chuỗi. Vì vậy, trò chơi về cơ bản là về số lượng nước đi có thể được chơi chứ không phải về các mẫu cụ thể. 

Các trường hợp khó khăn phá vỡ lối suy nghĩ ngây thơ xuất phát từ việc tập trung quá nhiều vào cấu trúc thay vì tính chẵn lẻ. 

Ví dụ, hãy xem xét một chuỗi như`01`. Alice loại bỏ nó ngay lập tức và giành chiến thắng. Vì`1111`, Alice loại bỏ một cặp, Bob loại bỏ một cặp và Alice không di chuyển. Vì`0011`, Alice loại bỏ một cặp, để lại`01`, và Bob xóa nó đi, để trống. Alice sau đó không còn di chuyển và thua cuộc. Những ví dụ này cho thấy tính chẵn lẻ của các nước đi chứ không phải cấu trúc cục bộ quyết định kết quả. 

Một trực giác sai lầm phổ biến là nghĩ rằng việc luân phiên các ký tự hoặc nhóm đều quan trọng. Trong thực tế, mỗi bước di chuyển sẽ giảm độ dài đi 2, do đó, số bước di chuyển về cơ bản được cố định bằng số lượng cặp liền kề rời rạc có thể được loại bỏ cho đến khi chuỗi trở nên quá ngắn. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô phỏng đầy đủ trạng thái trò chơi. Ở mỗi bước, người chơi thử mọi cách loại bỏ cặp liền kề có thể, đánh giá đệ quy chuỗi kết quả và xác định xem bất kỳ động thái nào có dẫn đến vị trí thua cho đối thủ hay không. Đây là tìm kiếm minimax tiêu chuẩn trên chuỗi. 

Vấn đề là số lượng các bang tăng lên cực kỳ nhanh chóng. Ngay cả đối với độ dài 20, hệ số phân nhánh lên tới 19 và các trạng thái lặp lại nhiều nhưng không theo cách có cấu trúc trừ khi việc ghi nhớ được thiết kế cẩn thận trên các chuỗi. Đối với chiều dài lên tới 100, phương pháp này trở nên hoàn toàn không khả thi. 

Quan sát quan trọng là mọi bước di chuyển luôn giảm độ dài chính xác bằng 2 và không có bước di chuyển nào thay đổi bất kỳ điều gì khác có liên quan đến khả năng di chuyển trong tương lai ngoại trừ việc thu nhỏ chuỗi. Điều đó có nghĩa là tổng số lần di chuyển có thể được xác định chỉ bằng số lần chúng ta có thể loại bỏ các cặp liền kề cho đến khi độ dài chuỗi trở thành 0 hoặc 1. 

Vì vậy, trò chơi giảm xuống việc đếm tổng cộng có bao nhiêu nước đi và kiểm tra xem Alice có thực hiện nước đi cuối cùng hay không. Nếu tổng số nước đi là số lẻ thì Alice thắng. Nếu hòa thì Bob thắng. 

Chúng tôi thậm chí không cần phải tính toán rõ ràng các bước di chuyển. Số lần di chuyển chỉ đơn giản là`len(s) // 2`bởi vì mỗi nước đi sẽ xóa chính xác hai ký tự và không có nước đi nào phụ thuộc vào cặp nào được chọn để xác định số nước đi cuối cùng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (Minimax trên các tiểu bang) | O(số mũ) | O(n) | Quá chậm | 
| Tối ưu (quan sát chẵn lẻ) | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Thuật toán dựa vào việc giảm trò chơi thành một phép kiểm tra tính chẵn lẻ đơn giản. 

1. Đọc chuỗi và tính độ dài của nó. Cấu trúc của các ký tự không liên quan đến quyết định cuối cùng nên chúng tôi bỏ qua nội dung thực tế. 
2. Xác định có thể thực hiện được bao nhiêu nước đi. Mỗi lần di chuyển sẽ loại bỏ chính xác hai ký tự, vì vậy số lần di chuyển là số nguyên chia cho 2. 
3. Quyết định người chiến thắng dựa trên tính chẵn lẻ. Nếu số nước đi là số lẻ, Alice thực hiện nước đi cuối cùng và Bob không có nước đi hợp lệ. Nếu hòa, Bob thực hiện nước đi cuối cùng và Alice thua sau đó. 
4. Xuất "DA" nếu Alice thắng và "NET" nếu ngược lại. 

Lý do mức giảm này hợp lệ là vì không có động thái nào thay đổi tính chẵn lẻ của cấu trúc có sẵn ngoài việc giảm độ dài một lượng cố định. Không có hiệu ứng phân nhánh nào có thể tăng hoặc giảm tổng số nước đi tùy theo chiến lược, bởi vì mỗi nước đi tiêu tốn chính xác hai ký tự bất kể cặp liền kề nào được chọn. 

### Tại sao nó hoạt động 

Điều bất biến là sau bất kỳ chuỗi nước đi hợp lệ nào, độ dài chuỗi còn lại luôn bằng`n - 2k`Ở đâu`k`là số lần di chuyển được thực hiện Trò chơi kết thúc chính xác khi giá trị này nhỏ hơn 2. Do đó, số nước đi tối đa có thể được cố định là`n // 2`, độc lập với sự lựa chọn. Vì cả hai người chơi luân phiên nhau di chuyển bắt đầu bằng Alice, Alice thắng khi và chỉ khi cô ấy thực hiện được nước đi cuối cùng trong chuỗi cố định này, tương đương với`n // 2`trở nên kỳ quặc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    s = input().strip()
    moves = len(s) // 2
    if moves % 2 == 1:
        print("DA")
    else:
        print("NET")
```Mã đọc từng trường hợp kiểm thử và ngay lập tức giảm nó thành một phép tính độ dài. Phép tính duy nhất cần thực hiện là chia số nguyên cho 2, tương ứng với tổng số lần xóa có thể. Việc kiểm tra tính chẵn lẻ trực tiếp xác định người chiến thắng. 

Một lỗi phổ biến là cố gắng mô phỏng việc xóa hoặc theo dõi các cặp thực tế. Điều đó là không cần thiết vì trò chơi không bao giờ phụ thuộc vào cặp cụ thể nào bị loại bỏ, mà chỉ phụ thuộc vào số lần loại bỏ xảy ra trước khi cạn kiệt. 

## Ví dụ đã hoạt động 

### Ví dụ 1:`01`| Bước | Độ dài chuỗi | Di chuyển được thực hiện | Người chơi tiếp theo | 
| --- | --- | --- | --- | 
| 0 | 2 | 0 | Alice | 
| 1 | 0 | 1 | Bob (không di chuyển) | 

Alice thực hiện đúng một nước đi, điều này thật kỳ lạ, vì vậy cô ấy thắng. 

Điều này xác nhận rằng trong chiều dài 2, Alice luôn thắng vì cô ấy thực hiện nước đi duy nhất có thể. 

### Ví dụ 2:`0011`| Bước | Độ dài chuỗi | Di chuyển được thực hiện | Người chơi tiếp theo | 
| --- | --- | --- | --- | 
| 0 | 4 | 0 | Alice | 
| 1 | 2 | 1 | Bob | 
| 2 | 0 | 2 | Alice (không di chuyển) | 

Ở đây có đúng 2 nước đi. Bob thực hiện nước đi thứ hai, khiến Alice không có nước đi hợp lệ. Vì số nước đi là chẵn nên Alice thua. 

Điều này chứng tỏ kết quả cuối cùng chỉ phụ thuộc vào tổng số lần xóa là lẻ hay chẵn chứ không phụ thuộc vào việc sắp xếp các ký tự. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi trường hợp thử nghiệm chỉ quét chuỗi một lần để tính độ dài của nó | 
| Không gian | O(1) | Không có cấu trúc bổ sung nào được lưu trữ ngoài quầy | 

Giải pháp phù hợp thoải mái trong các ràng buộc vì tổng kích thước đầu vào nhỏ và mỗi trường hợp kiểm thử được xử lý trong thời gian không đổi sau khi đọc. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    input = _sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        s = input().strip()
        moves = len(s) // 2
        out.append("DA" if moves % 2 == 1 else "NET")
    return "\n".join(out)

# provided samples
assert run("3\n01\n1111\n0011\n") == "DA\nNET\nNET"

# minimum size
assert run("1\n1\n") == "NET"

# simple win
assert run("1\n00\n") == "DA"

# alternating pattern
assert run("1\n0101\n") == "NET"

# maximum-ish small test
assert run("1\n" + "01"*50 + "\n") == "NET"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`NET`| ký tự đơn, không di chuyển | 
|`00`|`DA`| trường hợp thắng nhỏ nhất | 
|`0101`|`NET`| mẫu xen kẽ có độ dài chẵn | 
|`01*50`|`NET`| cấu trúc đồng đều lớn hơn, tính nhất quán chẵn lẻ | 

## Vỏ cạnh 

Đối với một nhân vật như`0`, thuật toán tính toán`len = 1`, Vì thế`moves = 0`và xuất ra "NET". Điều này phù hợp với thực tế là Alice không có nước đi hợp lệ nào ngay lập tức. 

Đối với một cặp như`01`,`moves = 1`, điều này thật kỳ lạ, vì vậy Alice thắng. Khi thực hiện, Alice loại bỏ cặp duy nhất và Bob không thể di chuyển. 

Đối với một chuỗi như`1111`, thuật toán tính toán`moves = 2`. Alice thực hiện nước đi đầu tiên, Bob thực hiện nước đi thứ hai và Alice không còn nước đi nào nữa. Tỷ lệ chẵn lẻ dự đoán chính xác Alice thua. 

Đối với các chuỗi xen kẽ như`010101`, chỉ có độ dài là quan trọng. Mặc dù tồn tại nhiều lệnh loại bỏ khác nhau nhưng thuật toán vẫn mang lại kết quả`moves = 3`, nghĩa là Alice thắng vì cô ấy đi được nước đi cuối cùng.
