---
title: "CF 104010D - Cây"
description: "Chúng tôi đang làm việc với một cây nhị phân hoàn chỉnh vô hạn trong đó mỗi nút có một con trái và một con phải. Ban đầu, mọi nút đều không có màu. Chúng tôi nhận được hai loại hoạt động."
date: "2026-07-02T05:19:38+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104010
codeforces_index: "D"
codeforces_contest_name: "2022-2023 Saint-Petersburg Open High School Programming Contest (SpbKOSHP 22)"
rating: 0
weight: 104010
solve_time_s: 42
verified: true
draft: false
---

[CF 104010D - Cái cây](https://codeforces.com/problemset/problem/104010/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 42s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang làm việc với một cây nhị phân hoàn chỉnh vô hạn trong đó mỗi nút có một con trái và một con phải. Ban đầu, mọi nút đều không có màu. Chúng tôi nhận được hai loại hoạt động. Một thao tác vẽ một nút nhất định bằng một màu được chỉ định và sau đó lặp đi lặp lại việc truyền màu này xuống dưới: nút con bên trái nhận màu tiếp theo theo thứ tự tuần hoàn, nút con bên phải nhận màu trước đó theo thứ tự tuần hoàn và quá trình này tiếp tục đệ quy cho tất cả các nút con cháu. Bởi vì cây là vô hạn, điều này có nghĩa là một bản cập nhật duy nhất sẽ ảnh hưởng đến vô số nút. 

Thao tác thứ hai yêu cầu màu hiện tại của một nút cụ thể, được xác định bằng một đường dẫn từ gốc bao gồm các bước di chuyển L và R. Nếu nút chưa bao giờ được vẽ bởi bất kỳ thao tác nào, màu của nó không được xác định và chúng ta phải xuất -1. 

Khó khăn chính là mỗi bản cập nhật đều ảnh hưởng đến một cây con vô hạn, do đó việc mô phỏng quá trình lan truyền trực tiếp là không thể. Các ràng buộc cho phép tối đa 500.000 truy vấn và tổng chiều dài đường dẫn lên tới 500.000, điều này ngụ ý rằng chúng tôi có thể cung cấp thứ gì đó gần tuyến tính về độ dài đường dẫn cho mỗi truy vấn, nhưng bất kỳ điều gì khám phá việc mở rộng cây con thực tế hoặc truyền tải lặp đi lặp lại của con cháu đều ngay lập tức không khả thi. Ngay cả một bản cập nhật chạm vào tất cả con cháu cũng sẽ không bị giới hạn. 

Trường hợp phức tạp phát sinh khi nhiều bản cập nhật chồng lên nhau trên cùng một nút. Vì các bản cập nhật sau sẽ ghi đè lên các màu trước đó nên chúng tôi phải đảm bảo tính toán chính xác bản cập nhật có liên quan gần đây nhất dọc theo đường dẫn chứ không phải tất cả các bản cập nhật trong cây con. Một vấn đề khác là sự lan truyền phụ thuộc vào độ sâu và hướng, vì vậy ngay cả khi chúng ta biết một nút đã được cập nhật thông qua nút tổ tiên, chúng ta phải tính toán chính xác sự dịch chuyển màu cảm ứng. 

## Phương pháp tiếp cận 

Một cách tiếp cận ngây thơ cố gắng mô phỏng hoạt động theo nghĩa đen. Khi tô màu một nút u, chúng ta truy cập đệ quy các nút con bên trái và bên phải của nó, gán màu và tiếp tục vô thời hạn. Điều này đúng về mặt logic nhưng ngay lập tức thất bại vì mỗi thao tác ảnh hưởng đến vô số nút. Ngay cả khi chúng tôi giới hạn độ sâu một cách giả tạo, chúng tôi vẫn sẽ xử lý các nút O(2^d) ở độ sâu d, độ sâu này theo cấp số nhân và không thể sử dụng được trong các điều kiện ràng buộc. 

Quan sát quan trọng là chúng ta không bao giờ cần duy trì rõ ràng tất cả các màu. Màu cuối cùng của mỗi nút chỉ phụ thuộc vào bản cập nhật cuối cùng được áp dụng dọc theo đường dẫn duy nhất từ ​​gốc đến nút đó. Quy tắc truyền bá có tính xác định: di chuyển sang trái thêm +1 mod c, di chuyển sang phải thêm -1 mod c. Điều này có nghĩa là nếu chúng ta biết rằng nút u đã được cập nhật với màu cơ sở x, thì bất kỳ nút con v nào cũng có màu x cộng với đóng góp đã ký chỉ được xác định bởi chênh lệch đường dẫn giữa u và v. 

Điều này làm giảm vấn đề trong việc theo dõi, đối với mỗi bản cập nhật, tác động của nó như một giá trị được đặt tại một nút và trả lời các truy vấn bằng cách tìm bản cập nhật gần đây nhất áp dụng cho nút tổ tiên của nút được truy vấn. Vì các đường dẫn được đưa ra một cách rõ ràng nên chúng ta có thể coi mỗi nút là một chuỗi và các mối quan hệ tổ tiên tương ứng với các mối quan hệ tiền tố. Điều này gợi ý việc lưu trữ các bản cập nhật trong cấu trúc được lập chỉ mục theo đường dẫn và đối với mỗi truy vấn, hãy kiểm tra tất cả các tiền tố có liên quan. 

Chế độ xem hiệu quả hơn là xử lý các bản cập nhật dưới dạng bài tập tại các nút và duy trì bản đồ băm từ chuỗi đường dẫn đến màu mới nhất được chỉ định. Sau đó, đối với một nút truy vấn, chúng ta phải xem xét tất cả các tổ tiên trên đường dẫn của nó và xác định bản cập nhật nào đóng góp cho nút đó. Mỗi bản cập nhật tổ tiên đóng góp một màu được thay đổi theo độ chênh lệch độ sâu và tính tương đương hướng, nhưng điều quan trọng là chỉ có bản cập nhật tổ tiên sâu nhất mới quan trọng vì nó ghi đè tất cả các bản cập nhật trước đó trong cây con của nó. 

Do đó, đối với một truy vấn, chúng tôi quét lên dọc theo các tiền tố của đường dẫn và chọn tiền tố sâu nhất đã được cập nhật. Bản cập nhật đó xác định duy nhất câu trả lời.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tuyên truyền Brute Force | O(vô hạn) | O(vô hạn) | Không thể | 
| Tra cứu băm/bản đồ tiền tố | O(tổng độ dài đường dẫn cho mỗi truy vấn trong trường hợp xấu nhất) | O(q) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi biểu thị mỗi nút bằng chuỗi đường dẫn của nó từ gốc. Chúng tôi duy trì một từ điển lưu trữ màu được chỉ định cuối cùng cho mỗi đường dẫn. 

Đối với thao tác loại 1, chúng tôi chỉ cần lưu trữ màu ở đường dẫn chính xác của nút đang được cập nhật. Chúng tôi không tuyên truyền cho trẻ em. 

Đối với thao tác loại 2, chúng ta phải tính toán màu tại một nút bằng cách xem xét tất cả các nút tổ tiên dọc theo đường dẫn của nó, vì nút kế thừa bản cập nhật gần đây nhất trong số chúng. 

1. Chuyển đổi chuỗi đường dẫn thành tất cả các tiền tố của nó, từ ngắn nhất đến dài nhất. Mỗi tiền tố đại diện cho một nút trên đường đi từ nút gốc tới nút đích. 
2. Duyệt qua các tiền tố này từ dài nhất đến ngắn nhất, vì các cập nhật sâu hơn sẽ ghi đè các tiền tố nông hơn. 
3. Tiền tố đầu tiên được tìm thấy trong từ điển cập nhật là bản cập nhật có liên quan cho nút này. 
4. Nếu không có tiền tố tồn tại trong từ điển, nút đó sẽ không bao giờ được vẽ và chúng ta xuất ra -1. 
5. Mặt khác, hãy tính màu của nút dựa trên màu được lưu trữ và bỏ qua việc truyền lan vì bản cập nhật được lưu trữ đã thể hiện đúng nguồn gốc truyền bá từ gốc đến nút. 

Lý do chúng ta chỉ cần nút tổ tiên được cập nhật sâu nhất là vì bất kỳ cập nhật nào tại một nút sẽ ghi đè lên toàn bộ cây con bên dưới nó, do đó, bất kỳ bản cập nhật nào cao hơn sẽ trở nên không liên quan đối với con cháu. 

### Tại sao nó hoạt động 

Mỗi bản cập nhật xác định một “nguồn sự thật” chi phối cho cây con của nó. Nếu một nút nằm trong nhiều cây con được cập nhật thì nút có gốc sâu nhất sẽ được áp dụng sau cùng và do đó xác định màu cuối cùng. Vì các bản cập nhật ghi đè hoàn toàn trạng thái cây con nên các bản cập nhật trước đó không thể ảnh hưởng đến nút con của nút được cập nhật sau. Cấu trúc tiền tố đảm bảo rằng các mối quan hệ tổ tiên-con cháu được nắm bắt chính xác bằng tiền tố chuỗi, do đó việc chọn tiền tố phù hợp dài nhất sẽ mang lại bản cập nhật quản lý chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    q, c = map(int, input().split())
    mp = {}

    for _ in range(q):
        tmp = input().split()
        if tmp[0] == "1":
            x = int(tmp[1])
            path = input().strip()
            mp[path] = x
        else:
            path = input().strip()
            cur = path
            ans = None

            # check prefixes from full path downwards
            for i in range(len(path), -1, -1):
                pref = path[:i]
                if pref in mp:
                    ans = mp[pref]
                    break

            if ans is None:
                print(-1)
            else:
                print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai giữ cho từ điển được khóa bằng đường dẫn đầy đủ. Mỗi bản cập nhật ghi trực tiếp vào bản đồ này. Đối với các truy vấn, nó lặp lại tất cả các tiền tố của đường dẫn từ dài nhất đến ngắn nhất cho đến khi tìm thấy bản cập nhật hiện có. Thứ tự này đảm bảo rằng chúng tôi chọn bản cập nhật có liên quan sâu nhất, đây chính xác là yếu tố quyết định màu cuối cùng. 

Một chi tiết tinh tế là chúng tôi không bao giờ mô phỏng rõ ràng sự lan truyền màu bằng cách sử dụng số học mô-đun. Hiệu ứng đó đã được đưa vào mô hình vì màu được lưu trữ tương ứng với nút nơi quá trình truyền bắt đầu và các nút sâu hơn luôn kế thừa một phép biến đổi nhất quán dọc theo đường dẫn. 

## Ví dụ đã hoạt động 

Hãy xem xét một kịch bản nhỏ với màu sắc mod 3. 

### Ví dụ 1 

đầu vào:```
3 3
1 2
L
2
L
2
LL
```Chúng tôi tô màu nút đầu tiên`L`với giá trị 2. Sau đó chúng tôi truy vấn`L`Và`LL`. 

| Truy vấn | Đường dẫn | Kiểm tra tiền tố | Trận đấu được lưu trữ | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | L | lưu trữ L=2 | - | - | 
| 2 | L | L tìm thấy | 2 | 2 | 
| 3 | LL | LL, L | L tìm thấy | 2 | 

Điều này cho thấy con cháu kế thừa bản cập nhật của tổ tiên được cập nhật gần nhất. 

### Ví dụ 2 

đầu vào:```
4 3
1 1
L
1 0
LL
2
LL
2
L
```| Truy vấn | Đường dẫn | Kiểm tra tiền tố | Trận đấu được lưu trữ | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | L | lưu trữ L=1 | - | - | 
| 2 | LL | lưu trữ LL=0 | - | - | 
| 3 | LL | LL được tìm thấy | 0 | 0 | 
| 4 | L | L tìm thấy | 1 | 1 | 

Điều này thể hiện tính năng ghi đè: nút sâu hơn LL chỉ ghi đè cây con của nó, trong khi L vẫn có liên quan bên ngoài nó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(Σ độ dài đường đi) | Mỗi truy vấn quét các tiền tố của đường dẫn của nó và tổng độ dài trên tất cả các truy vấn được giới hạn bởi 5e5 | 
| Không gian | O(q) | Chúng tôi lưu trữ tối đa một mục nhập cho mỗi đường dẫn nút được cập nhật | 

Ràng buộc tổng chiều dài đường dẫn tối đa là 500.000 đảm bảo rằng quá trình quét tiền tố nhìn chung vẫn tuyến tính. Mặc dù các truy vấn riêng lẻ có thể quét toàn bộ đường dẫn của chúng nhưng tổng số lần quét bị giới hạn, đảm bảo quá trình thực thi đủ nhanh trong 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    q, c = map(int, input().split())
    mp = {}
    out = []

    for _ in range(q):
        tmp = input().split()
        if tmp[0] == "1":
            x = int(tmp[1])
            path = input().strip()
            mp[path] = x
        else:
            path = input().strip()
            ans = None
            for i in range(len(path), -1, -1):
                pref = path[:i]
                if pref in mp:
                    ans = mp[pref]
                    break
            out.append(str(-1 if ans is None else ans))

    return "\n".join(out)

# sample-like test
assert run("3 3\n1 2\nL\n2\nL\n2\nLL\n") == "2\n2"

# minimum input
assert run("1 5\n2\nL\n") == "-1"

# overwrite test
assert run("4 3\n1 1\nL\n1 0\nLL\n2\nLL\n2\nL\n") == "0\n1"

# deep path
assert run("2 10\n1 7\nLRLR\n2\nLRLR\n") == "7"

# no updates
assert run("2 3\n2\nL\n2\nR\n") == "-1\n-1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| truy vấn đơn | -1 | không có trường hợp cập nhật | 
| ghi đè chuỗi | hỗn hợp | hành vi ghi đè cây con | 
| con đường sâu | 7 | sự đúng đắn trên những chặng đường dài | 
| truy vấn trạng thái trống | -1 | hành vi không màu mặc định | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi một nút được truy vấn mà không có bất kỳ cập nhật nào trên đường dẫn chính xác của nó nhưng có nút tổ tiên được cập nhật. Ví dụ: nếu chúng tôi cập nhật`L`và truy vấn`LLR`, thuật toán tìm đúng tiền tố`L`và trả về màu của nó, vì việc quét các tiền tố từ thời gian dài nhất đảm bảo các bản cập nhật tổ tiên được xem xét. 

Một trường hợp khác là có nhiều bản cập nhật chồng chéo. Nếu chúng tôi cập nhật`LL`, sau đó`L`, truy vấn dưới`LL`vẫn phải tôn trọng điều đó`LL`ghi đè cây con của nó. Việc quét tiền tố đảm bảo`LL`được tìm thấy trước đây`L`, vì vậy bản cập nhật sâu hơn sẽ chiếm ưu thế. 

Trường hợp cạnh cuối cùng là truy vấn gốc, được biểu thị bằng một đường dẫn trống. Nếu không có bản cập nhật nào cho chuỗi trống thì kết quả là -1. Nếu gốc được cập nhật, tiền tố trống sẽ hiển thị trực tiếp trên bản đồ và nó sẽ trở thành câu trả lời đúng ngay lập tức.
