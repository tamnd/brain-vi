---
title: "CF 103069K - Allin"
description: "Chúng ta được đưa ra một tình huống đối đầu đơn giản ở Texas hold’em chỉ với hai người chơi và không bỏ bài. Mỗi trường hợp thử nghiệm cung cấp năm lá bài: hai lá bài riêng cho Gà Sói và ba lá bài chung đại diện cho thất bại."
date: "2026-07-04T01:01:44+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103069
codeforces_index: "K"
codeforces_contest_name: "2020 ICPC Asia East Continent Final"
rating: 0
weight: 103069
solve_time_s: 46
verified: true
draft: false
---

[CF 103069K - Allin](https://codeforces.com/problemset/problem/103069/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được đưa ra một tình huống đối đầu đơn giản ở Texas hold’em chỉ với hai người chơi và không bỏ bài. Mỗi trường hợp thử nghiệm cung cấp năm lá bài: hai lá bài riêng cho Gà Sói và ba lá bài chung đại diện cho thất bại. Còn hai quân bài chung và hai quân bài riêng của đối phương vẫn chưa rõ. 

Câu hỏi không phải là mô phỏng một trò chơi poker thực tế mà là để quyết định điều gì đó mạnh mẽ hơn: liệu Gà Sói có thể đảm bảo chiến thắng bất kể bốn lá bài còn lại được chọn như thế nào từ bộ bài không nhìn thấy được hay không. Nếu tồn tại dù chỉ một khả năng hoàn thành bàn cờ và ván bài của đối thủ mà cho phép đối thủ hòa hoặc đánh bại Gà Sói, thì Gà Sói không được ăn tất tay. Chỉ khi thông tin hiện tại của Gà Sói đã buộc phải giành chiến thắng chắc chắn ở mọi trạng thái có thể xảy ra trong tương lai thì chúng ta mới trả lời rằng anh ta có thể toàn lực. 

Điều này biến vấn đề thành một cuộc kiểm tra ưu thế trong trường hợp xấu nhất đối với tất cả các lần hoàn thành bài toán đánh giá bài poker 7 lá. 

Các ràng buộc là cực kỳ lớn, lên tới 100000 trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm là đầu vào có kích thước không đổi, vì vậy giải pháp phải là O(1) cho mỗi trường hợp sau khi xử lý trước. Bất kỳ nỗ lực nào để liệt kê các lá bài chung hoặc ván bài của đối thủ có thể ngay lập tức là không thể bởi vì có hàng chục nghìn kết hợp ngay cả đối với một trường hợp thử nghiệm. 

Khó khăn chính không phải là đánh giá các ván bài poker mà là lý luận về tính chắc chắn khi đối thủ hoàn thành thông tin ẩn. 

Một vài trường hợp phức tạp minh họa tại sao lý luận ngây thơ lại thất bại. Nếu Gà Sói đã có một ván bài mạnh như bài thẳng hoặc bài tuôn ra từ các quân bài flop cộng lỗ, thì nó vẫn không tự động thắng vì đối thủ có thể tạo ra một ván bài thậm chí còn mạnh hơn tùy thuộc vào những quân bài chưa biết. Ví dụ: có một ván bài hòa hoặc thậm chí một ván bài hoàn thành không đảm bảo chiến thắng nếu đối thủ vẫn có thể đánh thẳng với các quân bài còn lại. 

Một trường hợp khác là ngôi nhà đầy đủ và tiềm năng của bốn loại. Ngay cả khi Gà Sói hiện đang tổ chức các chuyến đi, những lá bài chưa biết còn lại có thể cho phép đối thủ hoàn thành bộ tứ, điều này hoàn toàn đánh bại một nhà cái đầy đủ. Vì vậy, đánh giá cục bộ về cường độ hiện tại là không đủ. 

Thử thách thực sự là nhận ra khi nào các lá bài hiển thị đã buộc một cấu hình poker mạnh nhất có thể “bị khóa” mà không thể vượt qua được khi hoàn thành các lá bài còn lại. 

## Phương pháp tiếp cận 

Giải pháp bạo lực trực tiếp sẽ cố gắng mô phỏng tất cả các kết quả có thể xảy ra của hai lá bài chung còn lại và tất cả các lá bài tẩy có thể có của đối thủ được rút từ bộ bài còn lại. Đối với mỗi lần hoàn thành, chúng tôi sẽ đánh giá bộ bài năm lá bài tốt nhất của cả hai người chơi trên bảy lá bài và kiểm tra xem Gà Sói có luôn thắng hay không. 

Cách tiếp cận này đúng về mặt khái niệm nhưng hoàn toàn không khả thi. Sau khi loại bỏ 5 lá bài đã biết, còn lại 47 lá bài chưa biết. Chúng ta phải chọn 4 trong số đó cho đối thủ và cộng đồng tương lai, dẫn đến sự bùng nổ tổ hợp lên tới hàng trăm triệu trường hợp cho mỗi lần kiểm tra. Ngay cả khi cắt tỉa mạnh mẽ, việc đánh giá sức mạnh poker cho từng cấu hình vẫn vượt xa mọi giới hạn hợp lý. 

Quan sát quan trọng là “sự chắc chắn chiến thắng” là cực kỳ hiếm trong poker và chỉ xảy ra khi Gà Sói đã có cấu trúc tối đa không thể bị phá vỡ bởi bất kỳ trận hòa nào trong tương lai. Cách duy nhất để đảm bảo chiến thắng là đã nắm giữ được bộ bài hoàng gia và quan trọng hơn là đảm bảo rằng không có cấu hình lá bài không xác định nào có thể tạo ra bộ bài hoàng gia có thứ hạng ngang bằng hoặc cao hơn cho đối thủ. 

Tuy nhiên, vì chất bài không được chia sẻ và các quân bài là duy nhất, nếu Gà Sói đã có bộ bài hoàng gia được hình thành hoàn toàn từ những lá bài đã biết (lỗ + thất bại), thì không có quân bài nào trong tương lai có thể cải thiện ván bài của đối thủ vượt quá cấp độ đó, bởi vì bộ bài hoàng gia là loại ván bài tối đa tuyệt đối và được xác định trên một cấu trúc bộ đồ cụ thể không thể sao chép nếu không có chính xác các quân bài bị thiếu.

Do đó, vấn đề nằm ở việc kiểm tra xem Gà Sói đã hoàn thành bộ bài hoàng gia hay chưa bằng cách sử dụng hai quân bài tẩy của mình cộng với ba quân bài thất bại. 

Nếu có thì câu trả lời là “allin”. Ngược lại, câu trả lời là "kiểm tra", bởi vì trong tất cả các trường hợp khác đều tồn tại ít nhất một lần hoàn thành mà đối thủ có thể sánh hoặc đánh bại ván bài tốt nhất có thể. 

Sự giảm bớt này rất mạnh mẽ vì nó tránh được việc suy luận về tất cả các loại tay. Thay vào đó, chúng tôi xác định rằng chỉ có tay trên tuyệt đối mới có đặc tính chắc chắn khi hoàn thành đối thủ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê Brute Force của tất cả các lần hoàn thành | O(C(47,4) × đánh giá) | O(1) | Quá chậm | 
| Chỉ kiểm tra tuôn ra hoàng gia hiện có | O(1) mỗi lần kiểm tra | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi rút gọn từng trường hợp thử nghiệm để kiểm tra xem năm lá bài nhìn thấy được đã tạo thành bộ bài hoàng gia hay chưa. 

1. Chuyển đổi thứ hạng của mỗi quân bài thành một thang số trong đó Ten, Jack, Queen, King, Ace tương ứng với các giá trị cao liên tiếp theo thứ tự. Điều này cho phép so sánh trực tiếp và khớp mẫu mà không cần logic chuỗi. Bước này là cần thiết vì chúng ta chỉ quan tâm đến cấu trúc thứ hạng chính xác chứ không phù hợp với những tương tác vượt quá sự bình đẳng. 
2. Nhóm năm thẻ đã cho theo chất. Bộ bài hoàng gia phải nằm hoàn toàn trong một bộ đồ duy nhất, vì vậy bất kỳ giải pháp ứng cử viên nào cũng phải có tất cả năm lá bài có chung bộ đồ. 
3. Đối với mỗi nhóm chất, hãy thu thập các cấp bậc hiện có và kiểm tra xem bộ đó có chứa chính xác Mười, Jack, Hậu, Vua, Át hay không. Điều này xác minh cả tính đầy đủ và cấu trúc chính xác. 
4. Nếu bất kỳ chất nào thỏa mãn điều kiện này, ngay lập tức xuất ra “allin”, vì người chơi đã nắm giữ ván bài poker mạnh nhất có thể và không có sự kết hợp nào trong tương lai có thể vượt qua nó. 
5. Nếu không có chất nào thỏa mãn điều kiện, hãy xuất ra "kiểm tra", vì cấu hình hiện tại không buộc phải thắng được đảm bảo khi hoàn thành bất kỳ lá bài ẩn nào. 

Lý do điều này là đủ là vì thứ hạng của ván bài poker được sắp xếp nghiêm ngặt và thùng hoàng gia là yếu tố tối đa duy nhất. Bất kỳ ván bài nào chưa phải là ván bài hoàng gia đều có thể được cải thiện hoặc so khớp bằng cách hoàn thành các lá bài ẩn đối nghịch, vì vậy nó không thể đảm bảo một chiến thắng tuyệt đối. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

RANK_MAP = {
    '2': 2, '3': 3, '4': 4, '5': 5, '6': 6,
    '7': 7, '8': 8, '9': 9, 'T': 10,
    'J': 11, 'Q': 12, 'K': 13, 'A': 14
}

ROYAL = {10, 11, 12, 13, 14}

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        parts = input().split()
        suits = {}
        ranks_by_suit = {}

        for card in parts:
            r = RANK_MAP[card[0]]
            s = card[1]
            if s not in ranks_by_suit:
                ranks_by_suit[s] = set()
            ranks_by_suit[s].add(r)

        ok = False
        for s in ranks_by_suit:
            if ranks_by_suit[s] == ROYAL:
                ok = True
                break

        out.append("allin" if ok else "check")

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Mã đầu tiên mã hóa thứ hạng để so sánh nhất quán và nhanh chóng. Sau đó, nó tổng hợp các thẻ theo bộ bằng cách sử dụng các bộ để tránh các vấn đề trùng lặp và cho phép kiểm tra sự bình đẳng trực tiếp theo yêu cầu tuôn ra của hoàng gia. Kiểm tra cuối cùng là một so sánh tập hợp đơn giản, nắm bắt cả sự hiện diện và tính đầy đủ của năm cấp bậc được yêu cầu. 

Một điểm tinh tế là chúng ta không cần đảm bảo thứ tự hoặc tính liên tục một cách rõ ràng, bởi vì điều kiện tuôn ra hoàng gia xác định đầy đủ cả hai. 

## Ví dụ đã hoạt động 

Hãy xem xét trường hợp tất cả năm lá bài nhìn thấy được tạo thành một hình trái tim hoàng gia. 

| Bước | Đặt trái tim | 
| --- | --- | 
| Xử lý thẻ | {10, J, Q, K, A} | 
| Kiểm tra tình trạng | bằng HOÀNG GIA | 
| Kết quả | allin | 

Điều này chứng tỏ điều kiện thắng được phát hiện hoàn toàn bằng tập hợp đẳng thức. 

Bây giờ hãy xem xét trường hợp bộ đồ hỗn hợp trong đó chỉ tồn tại một phần của cấu trúc hoàng gia. 

| Bước | Bộ thuổng | 
| --- | --- | 
| Xử lý thẻ | {10, J, Q, A} | 
| Kiểm tra tình trạng | thiếu K | 
| Kết quả | kiểm tra | 

Điều này cho thấy các cấu trúc chưa hoàn thiện không thể đủ điều kiện ngay cả khi chúng trông gần giống với dòng hoàng gia. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(T) | Mỗi bài kiểm tra xử lý chính xác 5 thẻ với các thao tác liên tục trong thời gian | 
| Không gian | O(1) | Chỉ sử dụng các bộ và bản đồ có kích thước cố định cho mỗi bài kiểm tra | 

Giải pháp này dễ dàng nằm trong giới hạn vì ngay cả 100000 trường hợp thử nghiệm cũng chỉ liên quan đến vài trăm nghìn thao tác liên tục. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples
assert run("""2
AC KC QC JC TC
AC TD 8S 5H 2C
""") == """allin
check"""

# already a royal flush
assert run("""1
AH KH QH JH TH
""") == "allin"

# almost royal but missing one card
assert run("""1
AH KH QH JH 9H
""") == "check"

# mixed suits cannot form royal flush
assert run("""1
AS KH QH JH TH
""") == "check"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| xả hoàng gia | allin | phát hiện tích cực | 
| thiếu thứ hạng | kiểm tra | suýt bị từ chối | 
| bộ đồ hỗn hợp | kiểm tra | phù hợp với việc thực thi ràng buộc | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi tất cả năm lá bài đều có thứ hạng chính xác nhưng được chia thành các chất. Ví dụ: có Ten đến Ace nhưng không có trong một bộ đồ. 

đầu vào:```
AH KH QH JS TS
```Mặc dù tất cả các cấp bậc bắt buộc đều tồn tại nhưng chúng không được thống nhất theo bộ đồ, vì vậy không có cấp bậc hoàng gia nào tồn tại. Thuật toán nhóm chính xác theo sự phù hợp và không thực hiện được việc kiểm tra đẳng thức, tạo ra "kiểm tra". 

Một trường hợp đặc biệt khác là sự phân bổ thứ hạng giống như trùng lặp giữa các bộ đồ trong đó tồn tại nhiều mẫu hoàng gia một phần. Ngay cả khi đó, vì không có bộ đồ nào có đủ năm cấp bậc bắt buộc nên không xảy ra kết quả dương tính giả.
