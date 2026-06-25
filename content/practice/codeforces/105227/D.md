---
title: "CF 105227D - Đặt hàng trò chuyện"
description: "Chúng tôi đang mô phỏng một thanh bên trò chuyện động luôn hiển thị các cuộc trò chuyện gần đây trước tiên. Mỗi khi Polycarp gửi tin nhắn cho một người bạn, cuộc trò chuyện của người bạn đó sẽ trở thành hoạt động gần đây nhất và được chuyển lên đầu danh sách."
date: "2026-06-24T16:27:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105227
codeforces_index: "D"
codeforces_contest_name: "CPG Training Contest - 1"
rating: 0
weight: 105227
solve_time_s: 73
verified: false
draft: false
---

[CF 105227D - Đặt hàng trò chuyện](https://codeforces.com/problemset/problem/105227/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 13s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang mô phỏng một thanh bên trò chuyện động luôn hiển thị các cuộc trò chuyện gần đây trước tiên. Mỗi khi Polycarp gửi tin nhắn cho một người bạn, cuộc trò chuyện của người bạn đó sẽ trở thành hoạt động gần đây nhất và được chuyển lên đầu danh sách. Nếu người bạn đó chưa từng được liên hệ trước đó thì một mục trò chuyện mới sẽ được tạo ở trên cùng. Nếu người bạn đó đã tồn tại trong danh sách, mục nhập hiện tại của họ sẽ bị xóa khỏi vị trí hiện tại và được chèn lại ở phía trước. 

Đầu vào là một chuỗi tên người nhận trong các tin nhắn đặt hàng được gửi đi. Đầu ra là thứ tự cuối cùng của những người bạn duy nhất, từ tin nhắn gần đây nhất đến tin nhắn ít nhất, sau khi xử lý tất cả các thao tác. 

Các ràng buộc cho phép tối đa 200.000 tin nhắn và mỗi tên có độ dài tối đa là 10. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào liên tục quét hoặc thay đổi danh sách cho mỗi tin nhắn. Một cách tiếp cận đơn giản giúp loại bỏ một phần tử khỏi danh sách hoặc mảng Python ở giữa sẽ tốn thời gian tuyến tính cho mỗi thao tác, điều này sẽ dẫn đến hành vi bậc hai trong trường hợp xấu nhất. 

Một trường hợp phức tạp xuất hiện khi cùng một người bạn được nhắn tin nhiều lần. Ví dụ: nếu đầu vào là`a b a`, sau khi xử lý: 

- sau`a`, danh sách là`a`- sau đó`b`, danh sách là`b a`- sau đó`a`, danh sách trở thành`a b`Việc triển khai đơn giản chỉ cần chèn ở phía trước mà không loại bỏ sự xuất hiện cũ sẽ tạo ra các bản sao không chính xác như`a b a`, vi phạm yêu cầu mỗi người bạn xuất hiện tối đa một lần. 

Một trường hợp đặc biệt khác là khi tất cả tin nhắn đều được gửi đến cùng một người. Danh sách phải luôn kết thúc bằng chính xác một mục nhập, người đó, bất kể có bao nhiêu tin nhắn được gửi đi. 

## Phương pháp tiếp cận 

Mô phỏng lực lượng vũ phu giữ một danh sách các cuộc trò chuyện hiện tại. Đối với mỗi tên đến, nó sẽ quét danh sách để kiểm tra xem tên đó đã tồn tại chưa. Nếu có, nó sẽ xóa mục nhập đó, sau đó chèn tên ở phía trước. Nếu không, nó chỉ cần chèn ở phía trước. 

Điều này đúng vì nó phản ánh chính xác hành vi được mô tả. Tuy nhiên, mỗi thao tác yêu cầu tìm kiếm tuyến tính để tìm vị trí và một thao tác tuyến tính khác để loại bỏ hoặc dịch chuyển các phần tử. Trong trường hợp xấu nhất khi tất cả các thư đều khác biệt, chúng tôi liên tục quét một danh sách ngày càng dài ra, dẫn đến khoảng các phép toán 1 + 2 + 3 + ... + n, tức là O(n²). Với n lên tới 200.000, điều này là quá chậm. 

Quan sát quan trọng là chúng tôi chỉ quan tâm đến việc một tên có hiện diện hay không và vị trí hiện tại của nó, đồng thời chúng tôi cần cập nhật nhanh chóng cả khi tra cứu và sắp xếp lại. Điều này gợi ý việc kết hợp cấu trúc dựa trên hàm băm để theo dõi sự tồn tại với cấu trúc hỗ trợ chèn nhanh ở phía trước. Chỉ một bộ không thể duy trì trật tự và chỉ một danh sách thôi thì quá chậm để cập nhật thành viên. Giải pháp là duy trì một tập hợp dành cho thành viên và cấu trúc giống deque để đặt hàng, nhưng đơn giản hơn nữa, chúng ta có thể mô phỏng bằng cách sử dụng một tập hợp cộng với một danh sách được xây dựng ngược hoặc rõ ràng hơn, chúng ta xử lý từ đầu bằng cách sử dụng một tập hợp và xây dựng thứ tự cuối cùng một lần. 

Một cái nhìn sâu sắc đặc biệt rõ ràng là duyệt qua các thông điệp ngược lại. Khi quét từ tin nhắn cuối cùng đến tin nhắn đầu tiên, lần đầu tiên chúng ta gặp một cái tên thì đó phải là vị trí cuối cùng của đoạn chat đó trong câu trả lời. Bất kỳ lần xuất hiện nào trước đó đều không liên quan vì những lần xuất hiện sau sẽ ghi đè những lần xuất hiện trước đó trong quá trình chuyển tiếp. Vì vậy, chúng ta có thể xây dựng kết quả bằng cách lặp lại quá trình lặp lại và thu thập những cái tên được nhìn thấy lần đầu tiên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Quét ngược với Set | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý danh sách thư từ người nhận cuối cùng đến người nhận đầu tiên, duy trì một tập hợp tên đã có trong câu trả lời và danh sách cho thứ tự cuối cùng. 

1. Bắt đầu với một tập hợp trống và một danh sách trống. 

Tập hợp các bài hát mà bạn bè đã được xếp vào thứ tự cuối cùng. 
2. Lặp lại những người nhận tin nhắn từ cuối danh sách về đầu. 

Việc đảo ngược này có tác dụng vì các tin nhắn sau sẽ ghi đè các tin nhắn trước đó trong việc xác định thời điểm gần đây cuối cùng. 
3. Đối với mỗi tên, hãy kiểm tra xem nó đã có trong bộ chưa. 

Nếu nó đã hiện diện rồi thì chúng ta bỏ qua nó vì vị trí cuối cùng của nó đã được xác định. 
4. Nếu nó không có trong bộ, chúng tôi thêm nó vào danh sách kết quả và đánh dấu nó là đã thấy. 

Lần đầu tiên chúng ta gặp một cái tên theo thứ tự ngược lại tương ứng với lần xuất hiện gần đây nhất của nó trong chuỗi ban đầu. 
5. Tiếp tục cho đến khi tất cả tin nhắn được xử lý. 
6. Cuối cùng, đảo ngược danh sách đã xây dựng vì chúng tôi đã thu thập các tên theo thứ tự ngược lại với lần xuất hiện cuối cùng. 

### Tại sao nó hoạt động 

Vị trí cuối cùng của mỗi người bạn chỉ được xác định bởi lần xuất hiện cuối cùng của họ trong chuỗi đầu vào. Việc xử lý từ cuối đảm bảo rằng lần đầu tiên chúng ta gặp một tên chính xác là lần xuất hiện cuối cùng của nó trong thời gian tới. Bằng cách bỏ qua tất cả các lần gặp tiếp theo, chúng tôi đảm bảo mỗi người bạn được đưa vào chính xác một lần và theo đúng thứ tự gần đây. Tập hợp này thực thi tính duy nhất, trong khi truyền tải ngược lại mã hóa quy tắc “chiến thắng gần đây nhất”. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    arr = [input().strip() for _ in range(n)]
    
    seen = set()
    res = []
    
    for i in range(n - 1, -1, -1):
        name = arr[i]
        if name in seen:
            continue
        seen.add(name)
        res.append(name)
    
    print("\n".join(res))

if __name__ == "__main__":
    solve()
```Giải pháp đọc tất cả các tên trước để chúng ta có thể duyệt ngược lại. các`seen`set đảm bảo mỗi tên chỉ được xuất một lần. Chúng tôi nối các tên theo thứ tự đảo ngược cuối cùng và in trực tiếp chúng theo thứ tự đó, vì việc đảo ngược là không cần thiết nếu chúng tôi diễn giải chính xác cấu trúc: lần đầu tiên chúng tôi nhìn thấy một tên từ phía sau là vị trí cuối cùng của nó và chúng tôi nối thêm theo thứ tự đó. 

Chi tiết triển khai chính là loại bỏ các dòng đầu vào một cách hợp lý; không có nó, các ký tự dòng mới ở cuối sẽ phá vỡ các kiểm tra tính bằng nhau trong tập hợp. Ngoài ra, chúng tôi tránh mọi thao tác xóa hoặc chèn danh sách ở phía trước, điều này sẽ làm giảm hiệu suất. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`alex ivan roman ivan`Chúng tôi xử lý từ phải sang trái: 

| Bước | Tên | Đã thấy trước đây? | Hành động | Danh sách kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | Ivan | không | thêm | [ivan] | 
| 2 | la mã | không | thêm | [ivan, la mã] | 
| 3 | Ivan | vâng | bỏ qua | [ivan, la mã] | 
| 4 | alex | không | thêm | [ivan, roman, alex] | 

Đầu ra cuối cùng là`ivan roman alex`. 

Điều này xác nhận rằng chỉ có lần xuất hiện cuối cùng của mỗi tên mới quan trọng. 

### Ví dụ 2 

đầu vào:`alina maria ekaterina darya darya ekaterina maria alina`Chúng tôi lại quét từ phải sang trái: 

| Bước | Tên | Đã thấy trước đây? | Hành động | Danh sách kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | alina | không | thêm | [alina] | 
| 2 | maria | không | thêm | [alina, maria] | 
| 3 | ekaterina | không | thêm | [alina, maria, ekaterina] | 
| 4 | darya | không | thêm | [alina, maria, ekaterina, darya] | 
| 5 | darya | vâng | bỏ qua | giống nhau | 
| 6 | ekaterina | vâng | bỏ qua | giống nhau | 
| 7 | maria | vâng | bỏ qua | giống nhau | 
| 8 | alina | vâng | bỏ qua | giống nhau | 

Đầu ra:`alina maria ekaterina darya`. 

Điều này cho thấy các bản cập nhật lặp lại sẽ được thu gọn một cách chính xác thành một thứ tự cuối cùng duy nhất dựa trên lần xuất hiện cuối cùng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi tên được xử lý một lần, với các thao tác được đặt O(1) | 
| Không gian | O(n) | Lưu trữ danh sách đầu vào và đặt trong trường hợp xấu nhất | 

Độ phức tạp tuyến tính dễ dàng phù hợp với giới hạn 200.000 thao tác và mức sử dụng bộ nhớ vẫn nằm trong giới hạn do mỗi tên đều ngắn và được lưu trữ một lần. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque
    
    def solve():
        n = int(input())
        arr = [input().strip() for _ in range(n)]
        seen = set()
        res = []
        for i in range(n - 1, -1, -1):
            if arr[i] not in seen:
                seen.add(arr[i])
                res.append(arr[i])
        print("\n".join(res))
    
    old_stdout = sys.stdout
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue().strip()
    sys.stdout = old_stdout
    return out

# provided samples
assert run("4\nalex\nivan\nroman\nivan\n") == "ivan\nroman\nalex"
assert run("8\nalina\nmaria\nekaterina\ndarya\ndarya\nekaterina\nmaria\nalina\n") == "alina\nmaria\nekaterina\ndarya"

# custom cases
assert run("1\na\n") == "a"
assert run("3\na\na\na\n") == "a"
assert run("3\na\nb\na\n") == "a\nb"
assert run("5\na\nb\nc\nd\ne\n") == "e\nd\nc\nb\na"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 một | một | kích thước tối thiểu | 
| một một | một | ghi đè lặp đi lặp lại | 
| a b a | a b | sự thống trị xuất hiện lần cuối | 
| a b c d e | ed c b a | đặt hàng đầy đủ tính độc đáo | 

## Vỏ cạnh 

Đối với các tin nhắn giống hệt nhau lặp đi lặp lại, chẳng hạn như`a a a a`, thuật toán xử lý từ phía sau và ngay lập tức lấy dữ liệu đầu tiên`a`nó gặp, sau đó bỏ qua tất cả những cái khác. Bộ này ngăn chặn sự trùng lặp, vì vậy đầu ra vẫn là một`a`, phù hợp với yêu cầu rằng các cuộc trò chuyện phải là duy nhất. 

Đối với các mẫu xen kẽ như`a b a b a`, các cuộc gặp quét ngược`a`đầu tiên, sau đó`b`và bỏ qua những lần xuất hiện trước đó, tạo ra`a b`. Điều này trực tiếp phản ánh rằng chỉ có tin nhắn cuối cùng của mỗi người bạn mới xác định được thứ tự cuối cùng. 

Để tăng cường nghiêm ngặt các tên duy nhất, mỗi tên sẽ được thêm một lần theo thứ tự quét ngược, tạo ra kết quả đảo ngược của dữ liệu đầu vào, nhất quán với hành vi "gần đây nhất ở trên cùng" trong chuỗi tất cả các cuộc trò chuyện riêng biệt.
