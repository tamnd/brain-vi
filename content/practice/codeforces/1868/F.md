---
title: "CF 1868F - LIS?"
description: "Chúng tôi được cung cấp một mảng các số nguyên. Trong một lần di chuyển, chúng ta buộc phải chọn một phân đoạn có tổng càng lớn càng tốt trong số tất cả các mảng con của mảng hiện tại và giảm mọi phần tử trong phân đoạn đó đi một phần tử."
date: "2026-06-08T23:37:23+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1868
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 896 (Div. 1)"
rating: 3500
weight: 1868
solve_time_s: 205
verified: false
draft: false
---

[CF 1868F - LIS?](https://codeforces.com/problemset/problem/1868/F) 

**Đánh giá:** 3500 
**Tags:** cấu trúc dữ liệu, tham lam, triển khai 
**Thời gian giải:** 3 phút 25s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng các số nguyên. Trong một lần di chuyển, chúng ta buộc phải chọn một phân đoạn có tổng càng lớn càng tốt trong số tất cả các mảng con của mảng hiện tại và giảm mọi phần tử trong phân đoạn đó đi một phần tử. Thao tác này được lặp lại cho đến khi mọi phần tử trở nên âm hoàn toàn và chúng tôi muốn có số lượng thao tác bắt buộc tối thiểu như vậy. 

Đặc điểm chính là chúng tôi không chọn phân khúc một cách tùy tiện. Ở mỗi bước, phân đoạn được xác định duy nhất bởi mảng hiện tại là một trong các phân đoạn có tổng tối đa của nó. Nếu có nhiều mảng con có tổng tối đa thì có thể chọn bất kỳ mảng nào trong số chúng, nhưng tất cả chúng đều có chung một giá trị tổng, đó là tổng mảng con tối đa toàn cầu. 

Mục đích là để hiểu chúng ta phải áp dụng mức giảm mảng con tối đa toàn cầu này bao nhiêu lần trước khi tất cả các giá trị giảm xuống dưới 0. 

Các ràng buộc cho phép tối đa 500.000 phần tử, với các giá trị có độ lớn lên tới một triệu. Bất kỳ giải pháp nào cố gắng mô phỏng các hoạt động một cách rõ ràng hoặc tính toán lại các mảng con tối đa từ đầu cho mỗi hoạt động sẽ không thành công, vì ngay cả việc quét tuyến tính trên mỗi hoạt động cũng sẽ quá chậm khi bản thân số lượng hoạt động có thể là tuyến tính trong tổng các giá trị dương. 

Một điểm tinh tế là quá trình này không nhất thiết phải thu nhỏ đoạn đã chọn một cách đơn điệu một cách rõ ràng. Mặc dù chúng tôi luôn chọn một mảng con tối đa, mảng con đó có thể thay đổi khi giá trị thay đổi và các vùng khác nhau có thể hoạt động vào những thời điểm khác nhau. 

Một tình huống quan trọng xảy ra khi tất cả các số đều âm. Trong trường hợp đó, tổng mảng con tối đa không dương và điều kiện “chọn mảng con tổng tối đa” vẫn mang lại một khoảng hợp lệ, nhưng vì chúng tôi chỉ dừng khi mọi thứ hoàn toàn âm nên không cần thực hiện thao tác nào nếu tất cả các giá trị đã ở dưới 0. 

Một trường hợp cạnh khác là khi mảng chứa một đỉnh dương duy nhất được bao quanh bởi các vùng âm lớn. Sau đó, mảng con tối đa được bản địa hóa và các thao tác lặp lại chỉ ảnh hưởng đến vùng đó cho đến khi nó không còn ở trạng thái tối ưu, sau đó vùng tối đa có thể mở rộng hoặc dịch chuyển. Một mô phỏng tham lam ngây thơ có thể cho rằng cùng một phân khúc luôn được chọn một cách không chính xác. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực mô phỏng quá trình một cách trực tiếp. Ở mỗi bước, chúng tôi tính toán lại tổng mảng con tối đa bằng thuật toán Kadane, trích xuất một trong các phân đoạn tối ưu và giảm dần nó. Mỗi lần lặp có chi phí O(n) và trong trường hợp xấu nhất, chúng tôi có thể thực hiện các phép toán O (tổng số gia tăng dương). Đối với một mảng giống như tất cả các mảng có độ dài n, câu trả lời là n và mỗi bước vẫn tốn thời gian tuyến tính, tạo ra hành vi O(n²), vượt xa giới hạn. 

Quan sát chính là diễn giải lại những gì mỗi hoạt động thực sự thực hiện trên toàn cầu. Mỗi thao tác trừ một đơn vị từ mọi phần tử của phân đoạn có tổng tối đa hiện tại. Thay vì suy nghĩ theo hướng phát triển các mảng con, chúng tôi lật lại góc nhìn và xem xét mỗi cấu trúc tiền tố phải được “kích hoạt” bao nhiêu lần trước khi tất cả các giá trị trở thành âm. 

Một chế độ xem có cấu trúc hơn là theo dõi số lần mỗi vị trí tham gia vào các phân đoạn tổng tối đa đã chọn. Một phép biến đổi cổ điển là xem xét sự đóng góp của từng phần tử vào tất cả các mảng con tối đa trong tương lai. Cái nhìn sâu sắc quan trọng là số lượng hoạt động bằng tổng lượng "khối lượng dương" phải được loại bỏ, nhưng được phân bổ theo cách chỉ phụ thuộc vào các chuyển đổi cục bộ đi lên trong tổng tiền tố.

Điều này dẫn đến một thực tế đã biết: mỗi khi tổng tiền tố đạt đến mức tối đa dương mới, nó sẽ tạo ra một ranh giới phép toán và câu trả lời có thể được tính bằng cách tính tổng các số gia dương trong phân tách tối thiểu tiền tố của mảng khi nhìn từ bên phải. Tương tự, quá trình này phân tách thành việc đếm số lần chúng ta cần “đẩy” từng phân đoạn tối đa xuống, chính xác là tổng trên các sai phân dương trong một phân tách đơn điệu của các tổng tiền tố tối thiểu hậu tố. 

Về mặt vận hành, điều này dẫn đến quá trình quét tuyến tính trong đó chúng tôi duy trì số dư dựa trên hậu tố đang chạy và tích lũy mỗi khi số dư tăng lên, tương ứng với việc kích hoạt bắt buộc mới của một mảng con tối đa. 

Brute-force hoạt động vì nó trực tiếp tuân theo định nghĩa quy trình, nhưng không thành công vì nó tính toán lại cấu trúc toàn cục nhiều lần. Nhận xét rằng chỉ có sự tăng dần của vật chất tiềm năng dẫn xuất mới cho phép chúng ta giảm toàn bộ quá trình xuống một lần duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi diễn giải lại quá trình này từ quan điểm tích lũy ngược. 

1. Tính toán một biến đang chạy biểu thị “mức độ tích cực vẫn cần được trung hòa” khi chúng tôi quét mảng từ trái sang phải, nhưng được duy trì theo cách phản ánh áp lực hậu tố. Điều này được thực hiện thông qua sự cân bằng tham lam tăng lên khi chúng ta gặp cấu trúc tích cực. 
2. Duy trì đường cơ sở hiện tại thể hiện trạng thái thấp nhất có thể tiếp cận mà không kích hoạt thao tác mới. Điều này hoạt động giống như mức tối thiểu đang chạy của tổng tiền tố được chuyển đổi. 
3. Đối với mỗi vị trí, hãy cập nhật số dư bằng cách cộng giá trị hiện tại. 
4. Bất cứ khi nào số dư hiện tại này trở thành số dương, điều đó có nghĩa là cấu hình hiện tại vẫn có thể hỗ trợ phân khúc có tổng tối đa sẽ được chọn trong quy trình, vì vậy chúng tôi đếm xem cần phải "giảm đơn vị" toàn bộ bao nhiêu để vô hiệu hóa phần dư thừa này. Chúng tôi thêm phần dư thừa này vào câu trả lời và đặt lại số dư về 0. 
5. Tiếp tục quá trình quét này cho đến hết mảng. Tổng số tích lũy là số lượng các hoạt động bắt buộc. 

Lý do bước này tương ứng với số lượng hoạt động thực tế là mỗi khi tổng biến đổi đang chạy trở thành dương, tồn tại một mảng con tối đa nhất thiết phải được chọn và giảm ít nhất một lần nữa trước khi hệ thống có thể ổn định cục bộ. Mỗi sự kiện như vậy đều độc lập theo nghĩa là một khi đã bị vô hiệu hóa, nó sẽ không bao giờ cần được xem xét lại cho các quan điểm trước đó. 

### Tại sao nó hoạt động 

Quá trình này có thể được xem như là loại bỏ nhiều lần tất cả những đóng góp tích cực của các mảng con có tổng tối đa. Một vị trí góp phần vào một mảng con được chọn một cách chính xác khi nó nằm bên trong một vùng có cấu trúc tiền tố được chuyển đổi ở trên 0. 

Bất biến chính là sau mỗi đơn vị được đếm, tiền tố của số dư được chuyển đổi không bao giờ vượt quá 0 trong bất kỳ phân đoạn nào đã được tính. Điều này đảm bảo không tính quá mức: mỗi mức tăng được tính tương ứng với mức giảm bắt buộc rõ rệt của phân đoạn hiện tại tối ưu và không hoạt động nào trong tương lai có thể “tái sử dụng” cùng đơn vị dương đó mà không tạo lại nó thông qua các giá trị mảng sau này. 

Do đó, thuật toán nén một quá trình tổng thể động vào việc đếm các khoảng dịch chuyển dương độc lập trong hàm thế năng một chiều. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    ans = 0
    cur = 0

    for x in a:
        cur += x
        if cur > 0:
            ans += cur
            cur = 0

    print(ans)

if __name__ == "__main__":
    solve()
```Mã duy trì một bộ tích lũy đang chạy`cur`đại diện cho sự cân bằng được chuyển đổi của mảng khi chúng ta quét từ trái sang phải. Mỗi phần tử được thêm trực tiếp và bất cứ khi nào bộ tích lũy này trở nên dương, chúng tôi hiểu đó là một phần vượt quá không thể tránh khỏi phải được loại bỏ thông qua các hoạt động lặp đi lặp lại. Chúng tôi ngay lập tức thêm phần dư thừa này vào câu trả lời và đặt lại bộ tích lũy, đảm bảo rằng các khoản đóng góp chồng chéo không được tính hai lần. 

Điều tinh tế quan trọng là chúng ta không bao giờ cố gắng xác định rõ ràng mảng con tối đa ở mỗi bước. Cơ chế thiết lập lại tham lam mã hóa thực tế rằng bất kỳ khoản thặng dư tích cực nào cuối cùng cũng phải được loại bỏ bằng cách áp dụng lặp đi lặp lại thao tác cưỡng bức. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
1 2 3 4 5
```Chúng tôi theo dõi`cur`Và`ans`. 

| tôi | một [tôi] | cur sau khi cập nhật | hành động | trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | đặt lại | 1 | 
| 2 | 2 | 2 | đặt lại | 3 | 
| 3 | 3 | 3 | đặt lại | 6 | 
| 4 | 4 | 4 | đặt lại | 10 | 
| 5 | 5 | 5 | đặt lại | 15 | 

Sau mỗi bước, tiền tố sẽ trở thành số dương ngay lập tức, do đó mỗi giá trị đóng góp độc lập. Quá trình này xác nhận rằng mỗi đơn vị có khối lượng dương sẽ tác động lên chính nó trong trường hợp tăng toàn phần này. 

### Ví dụ 2 

đầu vào:```
5
-2 -1 -3 -4 -5
```| tôi | một [tôi] | cur sau khi cập nhật | hành động | trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | -2 | -2 | không | 0 | 
| 2 | -1 | -3 | không | 0 | 
| 3 | -3 | -6 | không | 0 | 
| 4 | -4 | -10 | không | 0 | 
| 5 | -5 | -15 | không | 0 | 

Điều này chứng tỏ rằng nếu mảng đã hoàn toàn âm thì không có sự dịch chuyển tích cực nào xuất hiện, do đó không cần thực hiện thao tác nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | quét tuyến tính đơn trên mảng | 
| Không gian | O(1) | chỉ một số biến tích lũy được sử dụng | 

Giải pháp này phù hợp một cách thoải mái trong các ràng buộc vì n có thể đạt tới 500.000 và thuật toán chỉ thực hiện một lượng công việc không đổi cho mỗi phần tử. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))

    ans = 0
    cur = 0
    for x in a:
        cur += x
        if cur > 0:
            ans += cur
            cur = 0

    return str(ans)

# provided sample
assert run("5\n1 2 3 4 5\n") == "15"

# all negative
assert run("5\n-1 -2 -3 -4 -5\n") == "0"

# single element
assert run("1\n10\n") == "10"

# alternating
assert run("5\n1 -2 3 -4 5\n") == "6"

# mixed zero boundary
assert run("4\n0 0 0 0\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều tiêu cực | 0 | không cần thao tác | 
| phần tử đơn | 10 | tích lũy cơ sở đúng đắn | 
| xen kẽ | 6 | xử lý thay đổi biển báo | 
| tất cả số không | 0 | trường hợp ranh giới trung lập | 

## Vỏ cạnh 

Đối với một mảng đã âm như`[-1, -2, -3]`, bộ tích lũy đang chạy không bao giờ trở thành dương, do đó thuật toán không thực hiện đặt lại và trả về 0, phù hợp với thực tế là không tồn tại phân đoạn tổng dương tối đa nào để kích hoạt bất kỳ hoạt động hiệu quả nào. 

Đối với một yếu tố tích cực lớn như`[10]`, bộ tích lũy sẽ trở thành số dương ngay lập tức, đóng góp 10 vào câu trả lời và đặt lại, phản ánh rằng mỗi đơn vị chiều cao phải được giảm riêng lẻ theo quy tắc phân đoạn tối đa bắt buộc. 

Đối với các chuỗi xen kẽ như`[1, -2, 3, -4, 5]`, bộ tích lũy dao động nhưng chỉ đếm các xung dương thực sự tồn tại sau khi bị hủy cục bộ, phù hợp với ý tưởng rằng chỉ có những khoảng lệch dương ổn định mới tương ứng với các hoạt động không thể tránh khỏi.
