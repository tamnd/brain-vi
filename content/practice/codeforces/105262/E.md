---
title: "CF 105262E - Tim Game"
description: "Chúng ta có một cây có gốc với nút 1 cố định làm gốc. Hai người chơi luân phiên di chuyển, bắt đầu với Đối tác bí mật. Một bước di chuyển bao gồm việc chọn bất kỳ nút nào ngoài nút gốc và xóa nút đó cùng với mọi nút trong cây con của nó."
date: "2026-06-24T02:33:04+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105262
codeforces_index: "E"
codeforces_contest_name: "Game of Coders 3.0"
rating: 0
weight: 105262
solve_time_s: 65
verified: true
draft: false
---

[CF 105262E - Trò chơi Tim](https://codeforces.com/problemset/problem/105262/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây có gốc với nút 1 cố định làm gốc. Hai người chơi luân phiên di chuyển, bắt đầu với Đối tác bí mật. Một bước di chuyển bao gồm việc chọn bất kỳ nút nào ngoài nút gốc và xóa nút đó cùng với mọi nút trong cây con của nó. Người chơi thực hiện nước đi hợp lệ cuối cùng sẽ thắng, nghĩa là người chơi phải đối mặt với một vị trí không còn nút gốc nào sẽ thua. 

Từ quan điểm mang tính cấu trúc hơn, mỗi bước di chuyển sẽ làm sụp đổ toàn bộ một cây con gốc và bản thân gốc đó không bao giờ được phép chọn hoặc xóa. Trò chơi kết thúc khi mọi nút ngoại trừ nút gốc đã bị xóa, trực tiếp hoặc gián tiếp thông qua việc xóa trước đó. 

Các ràng buộc cho phép tối đa 4 · 10^5 nút trong tất cả các trường hợp thử nghiệm, loại trừ mọi chiến lược cố gắng mô phỏng trạng thái trò chơi sau mỗi lần di chuyển. Ngay cả một mô phỏng đơn giản đơn giản tính toán lại các cây con hoặc cập nhật cấu trúc động cho mỗi lần di chuyển cũng sẽ quá chậm vì số lượng thao tác có thể tăng bậc hai trong trường hợp xấu nhất. Giải pháp dự định phải giảm trò chơi thành một thuộc tính có thể tính toán được theo thời gian tuyến tính cho mỗi trường hợp thử nghiệm, lý tưởng nhất là thứ gì đó chỉ phụ thuộc vào cấu trúc của cây theo cách rất thô. 

Một vấn đề tế nhị xuất hiện khi suy nghĩ cục bộ về các bước di chuyển. Một nút ở sâu trong cây chỉ loại bỏ cây con của chính nó, trong khi nút cao hơn sẽ loại bỏ phần lớn hơn của cây. Không rõ liệu việc thực hiện các nước đi “lớn” có lợi hay không hay lối chơi tối ưu thích loại bỏ tối thiểu. Việc giải quyết sự lựa chọn này là yếu tố quyết định toàn bộ kết quả của trò chơi. 

## Phương pháp tiếp cận 

Một cách trực tiếp để nghĩ về trò chơi là coi nó như một trò chơi khách quan tiêu chuẩn trên trạng thái cây. Mỗi vị trí là một khu rừng còn lại có gốc tại 1 và mỗi lần di chuyển sẽ xóa toàn bộ cây con. Người ta có thể cố gắng gán giá trị Sprague-Grundy cho mọi khu rừng có gốc có thể, nhưng không gian trạng thái rất lớn vì việc loại bỏ cây con sẽ thay đổi cấu trúc trên toàn cầu và không phân hủy hoàn toàn thành các trò chơi con độc lập. Bất kỳ nỗ lực nào để tính toán các giá trị Grundy một cách rõ ràng sẽ yêu cầu khám phá nhiều cấu hình theo cấp số nhân trong trường hợp xấu nhất, điều này ngay lập tức trở nên không khả thi. 

Sự đơn giản hóa chính đến từ việc quan sát những gì thực sự quan trọng về một động thái. Mỗi nước đi sẽ xóa ít nhất một nút và tùy thuộc vào đỉnh được chọn, nó có thể xóa nhiều nút khác. Quan sát mang tính cấu trúc quan trọng là việc xóa nhiều hơn mức cần thiết không bao giờ tạo ra những bước đi bổ sung trong tương lai mà chỉ loại bỏ các lựa chọn. Bản thân bất kỳ nút nào biến mất đều là một động thái tiềm năng, vì vậy việc xóa lớn sẽ làm giảm nghiêm trọng số lượng quyết định trong tương lai mà cả hai người chơi có thể đưa ra. 

Điều này biến vấn đề thành một quá trình đơn điệu: tài nguyên duy nhất được tiêu thụ là tập hợp các nút có thể tháo rời và mỗi lần di chuyển sẽ tiêu tốn ít nhất một trong số chúng. Nếu cả hai người chơi đều hành động tối ưu thì cả hai người đều không có động cơ để loại bỏ nhiều hơn mức cần thiết, vì làm như vậy chỉ rút ngắn trò chơi và giảm số lần họ phải di chuyển sau đó. 

Điều này dẫn đến sự giảm trung tâm: trong cách chơi tối ưu, mỗi nước đi có thể được coi là loại bỏ chính xác một nút. Một khi điều này được chấp nhận, toàn bộ trò chơi sẽ có độ dài xác định. Vì có chính xác n − 1 nút có thể tháo rời (tất cả trừ nút gốc), trò chơi kéo dài đúng n − 1 lượt di chuyển bất kể hình dạng cây. Do đó, người chiến thắng được xác định hoàn toàn bằng tính chẵn lẻ của n − 1. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng trò chơi đầy đủ / DP trên cây | Hàm mũ | Hàm mũ | Quá chậm | 
| Giảm chẵn lẻ (đối số loại bỏ lá tối ưu) | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Giải pháp hoàn toàn không cần mô phỏng các chuyển động; nó chỉ trích xuất kích thước của cây.

1. Đọc số nút n của test case. Điều này hoàn toàn xác định câu trả lời, vì cấu trúc cây trở nên không phù hợp một khi chúng ta chấp nhận hành vi chơi tối ưu. 
2. Quan sát rằng mỗi nước đi sẽ loại bỏ ít nhất một nút và trong cách chơi tối ưu, chúng ta có thể hạn chế sự chú ý vào các nước đi loại bỏ chính xác một nút. Lý do biện minh là việc loại bỏ các nút bổ sung chỉ loại bỏ các bước di chuyển trong tương lai mà nếu không sẽ kéo dài trò chơi. 
3. Vì trò chơi bắt đầu với n − 1 nút không phải gốc và mỗi nước đi có thể được coi là loại bỏ chính xác một trong số chúng, nên tổng số nước đi trong bất kỳ chuỗi chơi tối ưu nào đều cố định ở n − 1. 
4. Xác định người chiến thắng bằng cách kiểm tra tính chẵn lẻ của n − 1. Nếu n − 1 là số lẻ, người chơi đầu tiên (Đối tác bí mật) thực hiện nước đi cuối cùng và thắng. Nếu không, người chơi thứ hai (Eddard) sẽ thắng. 

### Tại sao nó hoạt động 

Bất biến đằng sau lời giải là đại lượng duy nhất có liên quan là số lượng nút không phải gốc còn lại. Mọi động thái hợp pháp đều làm giảm nghiêm ngặt số lượng này ít nhất một và bất kỳ động thái nào làm giảm số lượng này nhiều hơn một đều có thể được thay thế bằng một chuỗi các bước di chuyển trên các nút đã loại bỏ đó mà không làm thay đổi kết quả tối ưu. Sự tương đương này có nghĩa là tất cả các lượt chơi tối ưu có thể được chuyển thành các chuỗi trong đó chính xác một nút được loại bỏ mỗi lượt, duy trì thứ tự lượt và người chiến thắng trong khi tối đa hóa số lần di chuyển. Vì trạng thái cuối là cố định và không phụ thuộc vào chiến lược nên tính chẵn lẻ của số lượng nút có thể tháo rời ban đầu sẽ xác định người chiến thắng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        # read and discard edges
        for _ in range(n - 1):
            input()
        # game reduces to parity of n - 1 moves
        if (n - 1) % 2 == 1:
            out.append("The Secret Partner")
        else:
            out.append("Eddard")
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Mã phản ánh mức giảm trực tiếp. Tất cả các cạnh đều chỉ được đọc để sử dụng đầu vào; chúng không có vai trò gì trong tính toán. Giá trị có ý nghĩa duy nhất cho mỗi trường hợp thử nghiệm là n và quyết định là kiểm tra tính chẵn lẻ đơn giản trên n − 1. 

Một lỗi phổ biến ở đây là cố gắng xây dựng danh sách kề hoặc tính toán kích thước cây con, việc này gây tốn kém không cần thiết và gây rủi ro cho TLE khi bị ràng buộc hoàn toàn. 

## Ví dụ đã hoạt động 

Xét một cây nhỏ có n = 4. Có 3 nút không phải gốc nên độ dài của trò chơi là 3 nước đi. Kể từ khi Đối tác bí mật bắt đầu, họ thực hiện nước đi 1 và 3 nên sẽ thắng. 

| Xoay | Các nút không phải gốc còn lại | Giải thích hành động | 
| --- | --- | --- | 
| 1 (Đối tác bí mật) | 2 | loại bỏ một nút | 
| 2 (Eddard) | 1 | loại bỏ một nút | 
| 3 (Đối tác bí mật) | 0 | loại bỏ nút cuối cùng | 

Điều này xác nhận rằng với số lẻ n − 1, người chơi đầu tiên sẽ đi nước cuối cùng. 

Bây giờ hãy xem xét n = 5. Có 4 nút không phải là nút gốc nên sẽ xảy ra 4 lần di chuyển. 

| Xoay | Các nút không phải gốc còn lại | Giải thích hành động | 
| --- | --- | --- | 
| 1 (Đối tác bí mật) | 3 | loại bỏ một nút | 
| 2 (Eddard) | 2 | loại bỏ một nút | 
| 3 (Đối tác bí mật) | 1 | loại bỏ một nút | 
| 4 (Eddard) | 0 | loại bỏ nút cuối cùng | 

Ở đây người chơi thứ hai thực hiện nước đi cuối cùng nên họ thắng. 

Những dấu vết này minh họa rằng cấu trúc của cây không ảnh hưởng đến độ dài chuỗi, chỉ có số lượng nút ban đầu mới ảnh hưởng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi cạnh được đọc một lần; không xử lý ngoài mức tiêu thụ đầu vào | 
| Không gian | O(1) thêm | Chỉ sử dụng bộ đếm và bộ lưu trữ đầu ra | 

Tổng kích thước đầu vào trên tất cả các trường hợp thử nghiệm tối đa là 4 · 10^5, do đó, quét tuyến tính trên tất cả các cạnh dễ dàng phù hợp trong giới hạn thời gian. Không cần xử lý đệ quy hoặc tổ hợp. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque
    import sys

    input = sys.stdin.readline
    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        for _ in range(n - 1):
            input()
        if (n - 1) % 2 == 1:
            out.append("The Secret Partner")
        else:
            out.append("Eddard")
    return "\n".join(out)

# provided sample (single-node interpretation example)
assert run("1\n1\n") == "Eddard"

# minimal non-trivial tree (n=2)
assert run("1\n2\n1 2\n") == "The Secret Partner"

# chain of length 3 (n=3)
assert run("1\n3\n1 2\n2 3\n") == "Eddard"

# star-shaped tree (n=5)
assert run("1\n5\n1 2\n1 3\n1 4\n1 5\n") == "Eddard"

# even n case where first wins
assert run("1\n4\n1 2\n1 3\n1 4\n") == "The Secret Partner"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=1 | Eddard | trường hợp cạnh chỉ có gốc | 
| n=2 | Đối tác bí mật | trò chơi hoạt động nhỏ nhất | 
| n=3 | Eddard | lật chẵn lẻ | 
| sao n=5 | Eddard | độc lập về cấu trúc | 

## Vỏ cạnh 

Trường hợp một nút làm nổi bật ranh giới nơi không có chuyển động nào tồn tại. Với n = 1, không có lựa chọn hợp lệ nên Đối tác bí mật ngay lập tức thua cuộc. Công thức cho n − 1 = 0, là số chẵn, mang lại chiến thắng cho người chơi thứ hai một cách chính xác. 

Với n = 2, tồn tại đúng một nước đi. Đối tác Bí mật loại bỏ nút không phải gốc duy nhất, để lại nút gốc và kết thúc trò chơi nên họ thắng. Quy tắc chẵn lẻ cho n − 1 = 1, số lẻ, phù hợp với kết quả này. 

Trong các cây lớn hơn như cây sao, thực tế là tất cả các nút đều là con trực tiếp của gốc không thay đổi gì cả. Mặc dù về nguyên tắc, các bước di chuyển có thể loại bỏ số lượng nút khác nhau, nhưng cách chơi tối ưu sẽ giảm xuống việc loại bỏ một nút duy nhất và kết quả vẫn chỉ được xác định bằng tổng số nút có thể tháo rời là số lẻ hay số chẵn.
