---
title: "CF 463B - Caisa và giá treo"
description: "Chúng ta được cho một dãy các cột được sắp xếp thành một đường thẳng, bắt đầu từ vị trí 0 đến vị trí n. Tháp đầu tiên được cố định ở độ cao 0 và mọi tháp khác có chiều cao nhất định."
date: "2026-05-30T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 463
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 264 (Div. 2)"
rating: 1100
weight: 463
solve_time_s: 59
verified: true
draft: false
---

[CF 463B - Caisa và Giá treo](https://codeforces.com/problemset/problem/463/B) 

**Đánh giá:** 1100 
**Tags:** vũ lực, thực hiện, toán học 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy các cột được sắp xếp thành một đường thẳng, bắt đầu từ vị trí 0 đến vị trí n. Tháp đầu tiên được cố định ở độ cao 0 và mọi tháp khác có chiều cao nhất định. Một người chơi bắt đầu tại tháp 0 với năng lượng bằng 0 và di chuyển về phía trước, từng bước một, từ tháp k đến tháp k+1. 

Mỗi lần di chuyển sẽ thay đổi năng lượng bằng sự chênh lệch giữa độ cao hiện tại và độ cao tiếp theo. Nếu cột tiếp theo thấp hơn thì năng lượng tăng lên, nếu cao hơn thì năng lượng giảm đi. Hạn chế chính là năng lượng không bao giờ được phép giảm xuống dưới 0 tại bất kỳ thời điểm nào trong suốt hành trình. 

Chúng tôi được phép tăng chiều cao của bất kỳ cột tháp nào bằng cách chi một đô la cho mỗi đơn vị tăng lên và chúng tôi muốn giảm thiểu tổng số đô la chi tiêu để hành trình từ 0 đến n trở nên khả thi. 

Ràng buộc n 100000 ngay lập tức loại trừ mọi cách tiếp cận thử tất cả các sửa đổi độ cao có thể có hoặc mô phỏng tìm kiếm tốn kém trên các cấu hình. Bất kỳ giải pháp nào cũng phải tuyến tính hoặc gần tuyến tính trong n. Ngay cả O(n log n) cũng có thể chấp nhận được, nhưng mọi thứ bậc hai thì không. 

Một cách tiếp cận ngây thơ có thể cố gắng mô phỏng cuộc hành trình và bất cứ khi nào năng lượng trở nên tiêu cực, hãy tăng một số độ cao trước đó hoặc hiện tại một cách tham lam mà không có chiến lược rõ ràng. Điều này không thành công vì những điều chỉnh ban đầu ảnh hưởng đến quá trình chuyển đổi sau này theo những cách không cục bộ. 

Một trường hợp lỗi nhỏ xuất hiện khi các bản sửa lỗi cục bộ bị thiển cận. Ví dụ: nếu chiều cao là:```
n = 3
h = [5, 1, 5]
```Một mô phỏng tham lam có thể khắc phục tình trạng giảm từ 5 xuống 1, nhưng sau đó vẫn thất bại ở mức 1 xuống 5, dẫn đến việc nâng cấp dư thừa hoặc đặt sai vị trí. Giải pháp đúng đắn phải có lý do toàn cầu về mức thâm hụt tồi tệ nhất gặp phải trên đường đi chứ không phải phản ứng từng bước một. 

## Phương pháp tiếp cận 

Ý tưởng vũ lực là mô phỏng hành trình và bất cứ khi nào năng lượng trở nên âm, hãy thử tăng một số chiều cao tháp để bù lại. Người ta có thể thử mọi lựa chọn về nơi tiêu tiền ở mỗi điểm thất bại. Điều này nhanh chóng trở thành tổ hợp: mỗi lần điều chỉnh sẽ thay đổi những khác biệt trong tương lai và trong trường hợp xấu nhất, chúng ta có thể xem xét lại các quyết định trước đó nhiều lần. Với n lên tới 100000, thậm chí vài nghìn lần điều chỉnh phân nhánh cũng đủ để bùng nổ vượt quá giới hạn. 

Cái nhìn sâu sắc quan trọng là ngừng suy nghĩ về sự tiến hóa năng lượng và thay vào đó hãy viết lại những gì thực sự quan trọng. Mỗi nước đi đóng góp một sự khác biệt h[k] - h[k+1]. Tổng hợp những thứ này theo thời gian sẽ mang lại năng lượng tích lũy chỉ phụ thuộc vào hành vi tiền tố. Lần duy nhất năng lượng trở nên có vấn đề là khi tổng tiền tố của những khác biệt này giảm xuống dưới 0. 

Sắp xếp lại biểu thức cho thấy rằng những khoảnh khắc nguy hiểm duy nhất là khi độ cao h[i] lớn hơn bất cứ thứ gì chúng ta có thể chấp nhận được từ năng lượng tích lũy. Thay vì theo dõi năng lượng một cách rõ ràng, chúng ta có thể duy trì "ngân sách miễn phí" mà chúng ta có và đảm bảo nó không bao giờ bị âm. Lần duy nhất chúng ta cần phải trả tiền là khi mức giảm không thể bù đắp được mức tăng trước đó. 

Một cách giải thích đơn giản hơn xuất hiện: bất cứ khi nào chúng ta gặp một độ cao lớn hơn tất cả các độ cao hiệu quả trước đó, chúng ta phải "trả tiền" để nâng đường cơ sở của mình lên để có thể đạt tới nó mà không có năng lượng tiêu cực. Điều này giúp giảm bớt vấn đề trong việc theo dõi độ cao tối đa đang chạy và mức thâm hụt tích lũy so với mức ban đầu. 

Do đó, giải pháp tối ưu là tuyến tính: chúng tôi quét từ trái sang phải, duy trì chiều cao cao nhất được thấy cho đến nay và bất cứ khi nào chiều cao tiếp theo lớn hơn công suất hiệu quả hiện tại của chúng tôi, chúng tôi sẽ trả chính xác số tiền chênh lệch. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi diễn giải lại hành trình như việc duy trì độ cao cơ bản để đảm bảo năng lượng không âm. 

1. Bắt đầu với đường cơ sở bằng 0, vì chúng ta bắt đầu ở tháp 0 với chiều cao 0 và năng lượng bằng 0. Chúng tôi cũng khởi tạo câu trả lời là 0, biểu thị tổng số tiền đã chi tiêu. 
2. Lặp qua các giá trị treo từ 1 đến n. Ở mỗi bước, hãy so sánh chiều cao hiện tại với yêu cầu cơ bản. 
3. Nếu chiều cao hiện tại lớn hơn đường cơ sở, chúng ta phải tăng đường cơ sở để phù hợp với nó. Sự khác biệt thể hiện chính xác số đô la chúng ta phải chi vào thời điểm này, bởi vì nếu không nâng cao các cột tháp trước đó một cách hiệu quả, năng lượng sẽ trở nên âm khi bước vào cột tháp cao hơn này. 
4. Thêm sự khác biệt này vào câu trả lời. Mô hình này chi tiền để tăng một cột tháp phù hợp để việc chuyển đổi vẫn khả thi. 
5. Cập nhật đường cơ sở thành giá trị tối đa của giá trị hiện tại và chiều cao hiện tại. 
6. Tiếp tục cho đến khi tháp cuối cùng được xử lý. Chi phí tích lũy là mức chi tiêu tối thiểu cần thiết. 

### Tại sao nó hoạt động 

Bất biến quan trọng là sau khi xử lý tháp i, đường cơ sở bằng chiều cao tối đa được thấy cho đến nay. Điều này đảm bảo rằng tất cả các chuyển đổi được xử lý trước đó đều an toàn, nghĩa là không có tiền tố di chuyển nào có năng lượng âm. Bất cứ khi nào một chiều cao mới vượt quá mức tối đa này, cách duy nhất để tránh sự sụt giảm năng lượng tiêu cực ở bước đó là bù đắp nó bằng cách nâng cao một số cột tháp trước đó. Vì mỗi đơn vị tăng chi phí đúng một đô la bất kể nó được áp dụng ở đâu, nên việc tăng dần mức cơ sở tại thời điểm chúng tôi gặp mức tối đa mới là tối ưu và không bao giờ trả quá nhiều. Bất kỳ sự điều chỉnh bị trì hoãn nào vẫn cần phải phù hợp với cùng khoảng cách chiều cao, vì vậy sự tích lũy tham lam này là tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    h = list(map(int, input().split()))

    current_max = 0
    cost = 0

    for x in h:
        if x > current_max:
            cost += x - current_max
            current_max = x

    print(cost)

if __name__ == "__main__":
    solve()
```Việc triển khai duy trì chiều cao tối đa đang chạy cho đến nay. Bất cứ khi nào một chiều cao mới vượt quá nó, chúng tôi sẽ cộng thêm phần chênh lệch vào chi phí. Điều này trực tiếp mã hóa ý tưởng rằng chúng ta phải "nâng" địa hình trước đó lên để đạt được đỉnh mới. 

Một lỗi phổ biến là cố gắng mô phỏng năng lượng trực tiếp bằng cách sử dụng các khác biệt về tiền tố. Cách tiếp cận đó làm phức tạp vấn đề và đưa ra trạng thái không cần thiết. Phương pháp theo dõi tối đa tham lam tránh hoàn toàn việc theo dõi năng lượng rõ ràng. 

Một điểm tinh tế khác là chúng ta không bao giờ điều chỉnh bất cứ điều gì một cách rõ ràng trong một mảng. Lý do dựa trên sự tương đương: việc tăng cột tháp trước đó hoặc tăng đường cơ sở về mặt khái niệm sẽ tạo ra tác động tương tự đối với tất cả những khác biệt trong tương lai. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
3 4 3 2 4
```Chúng tôi theo dõi current_max và chi phí. 

| tôi | chiều cao | current_max trước | hành động | chi phí | current_max sau | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 3 | 0 | +3 | 3 | 3 | 
| 2 | 4 | 3 | +1 | 4 | 4 | 
| 3 | 3 | 4 | không | 4 | 4 | 
| 4 | 2 | 4 | không | 4 | 4 | 
| 5 | 4 | 4 | không | 4 | 4 | 

Dấu vết này cho thấy chỉ có những đỉnh mới cần chi tiêu. Mỗi khi chúng tôi thấy một giá trị cao hơn tất cả những giá trị trước đó, chúng tôi sẽ trả chính xác khoảng cách. 

### Ví dụ 2 

đầu vào:```
4
1 2 2 5
```| tôi | chiều cao | current_max trước | hành động | chi phí | current_max sau | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | +1 | 1 | 1 | 
| 2 | 2 | 1 | +1 | 2 | 2 | 
| 3 | 2 | 2 | không | 2 | 2 | 
| 4 | 5 | 2 | +3 | 5 | 5 | 

Điều này xác nhận rằng các giá trị lặp lại không kích hoạt chi tiêu, trong khi chỉ làm tăng vấn đề một cách nghiêm ngặt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lần truyền qua tất cả các giá treo, công việc liên tục trên mỗi phần tử | 
| Không gian | O(1) | Chỉ một số biến được sử dụng bất kể kích thước đầu vào | 

Giải pháp phù hợp thoải mái trong giới hạn vì n lên tới 100000 và thuật toán chỉ thực hiện các so sánh và bổ sung đơn giản cho mỗi phần tử. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n = int(input())
    h = list(map(int, input().split()))

    current_max = 0
    cost = 0

    for x in h:
        if x > current_max:
            cost += x - current_max
            current_max = x

    return str(cost)

# provided sample
assert run("5\n3 4 3 2 4\n") == "4"

# minimum input
assert run("1\n1\n") == "1"

# already decreasing sequence
assert run("4\n5 4 3 2\n") == "5"

# strictly increasing
assert run("4\n1 2 3 4\n") == "4"

# all equal
assert run("5\n2 2 2 2 2\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | 1 | xử lý trường hợp tối thiểu | 
| 5 4 3 2 | 5 | dãy giảm dần | 
| 1 2 3 4 | 4 | tăng tích lũy trình tự | 
| 2 2 2 2 2 | 2 | không chi tiêu lặp đi lặp lại | 

## Vỏ cạnh 

Một trường hợp tối thiểu như`n = 1, h = [1]`chứng tỏ rằng chúng ta vẫn phải chi tiêu khi cột tháp đầu tiên vượt quá 0. The algorithm correctly adds`1 - 0 = 1`ngay lập tức. 

Một chuỗi giảm nghiêm ngặt như`5 4 3 2`không bao giờ kích hoạt chi tiêu bổ sung sau bước đầu tiên. Giá trị đầu tiên đặt đường cơ sở bắt buộc và mọi thứ sau đó đều ở dưới giá trị đó một cách an toàn. Thuật toán xử lý việc này bằng cách cập nhật`current_max`đến 5 ở bước đầu tiên và không bao giờ thay đổi nữa, mang lại tổng chi phí là 5. 

Một dãy tăng nghiêm ngặt như`1 2 3 4`buộc phải chi tiêu ở mỗi bước vì mỗi độ cao mới đều vượt quá mức tối đa trước đó. Thuật toán bổ sung các khác biệt tích lũy: 1, rồi 1, rồi 1, rồi 1, phù hợp với nhu cầu trực quan là liên tục nâng cao đường cơ sở.
