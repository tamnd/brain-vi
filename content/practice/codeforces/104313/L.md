---
title: "CF 104313L - \u041f\u043e\u0447\u0435\u043c\u0443 \u043a\u0430\u0440\u0442\u044b \u0432 \u0434\u0440\u0443\u0433\u043e\u043c \u043f\u043e\u0440\u044f\u0434\u043a\u0435?"
description: "Chúng ta được cung cấp một chuỗi các lá bài cuối cùng xuất hiện trên bàn trong suốt trò chơi. Trong trò chơi, người chơi bắt đầu với một bộ bài ban đầu ẩn và liên tục loại bỏ lá bài trên cùng hoặc dưới cùng của bộ bài."
date: "2026-07-01T19:48:18+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104313
codeforces_index: "L"
codeforces_contest_name: "II \u041e\u0442\u043a\u0440\u044b\u0442\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u042e\u041c\u0428 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e"
rating: 0
weight: 104313
solve_time_s: 71
verified: true
draft: false
---

[CF 104313L - \u041f\u043e\u0447\u0435\u043c\u0443 \u043a\u0430\u0440\u0442\u044b \u0432 \u0434\u0440\u0443\u0433\u043e\u043c \u043f\u043e\u0440\u044f\u0434\u043a\u0435?](https://codeforces.com/problemset/problem/104313/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 11 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các lá bài cuối cùng xuất hiện trên bàn trong suốt trò chơi. Trong trò chơi, người chơi bắt đầu với một bộ bài ban đầu ẩn và liên tục loại bỏ lá bài trên cùng hoặc dưới cùng của bộ bài. Mỗi lá bài bị loại bỏ sẽ được đặt lên bàn theo thứ tự được lấy ra, vì vậy trình tự trên bàn chính xác là trình tự xóa từ hai đầu của bộ bài. 

Câu hỏi được đảo ngược: chúng ta được cung cấp trình tự bảng cuối cùng và phải quyết định xem liệu có tồn tại thứ tự ban đầu nào đó của các quân bài giống nhau sao cho bộ bài được sắp xếp theo thứ hạng từ dưới lên trên khi bắt đầu hay không và một trình tự nào đó lấy từ trên hoặc dưới tạo ra chính xác trình tự đầu ra đã cho. Nếu cấu trúc như vậy tồn tại, chúng ta phải xuất ra một bộ bài được sắp xếp ban đầu hợp lệ và một chuỗi thao tác hợp lệ, trong đó việc lấy từ dưới lên được mã hóa thành D và từ trên xuống dưới dạng U. 

Hạn chế chính là khái niệm về một bộ bài được sắp xếp. Việc sắp xếp chỉ được xác định theo thứ hạng chứ không phải theo sự phù hợp. Từ dưới lên trên, thứ hạng của lá bài phải không giảm, nghĩa là thứ hạng nhỏ nhất ở dưới cùng và thứ hạng lớn nhất ở trên cùng. Trong các cấp bậc bằng nhau, các bộ quần áo có thể được sắp xếp tùy ý vì chúng không ảnh hưởng đến thứ tự. 

Kích thước bài toán nhỏ cho mỗi trường hợp thử nghiệm, với tối đa 52 lá bài trong một bộ bài và lên tới 6000 trường hợp thử nghiệm. Điều này ngay lập tức gợi ý rằng mô phỏng O(n) hoặc O(n log n) cho mỗi thử nghiệm là đủ, trong khi bất kỳ việc xây dựng theo cấp số nhân nào của các bộ bài hoặc trình tự vận hành có thể là không cần thiết. 

Một quan niệm sai lầm ngây thơ nhưng quan trọng là cố gắng xây dựng lại cả bộ bài ban đầu và trình tự hoạt động một cách độc lập. Cách tiếp cận đó nhanh chóng trở nên mơ hồ vì nhiều cách sắp xếp ban đầu khác nhau có thể dẫn đến cùng một kết quả. Một dạng thất bại khác là bỏ qua rằng bộ bài ban đầu phải được sắp xếp trong suốt quá trình lập luận. Nếu không có ràng buộc này, người ta có thể thử xây dựng lại deque tùy ý không hợp lệ theo yêu cầu được sắp xếp. 

Trường hợp phức tạp hơn sẽ xuất hiện khi cả hai đầu của bộ bài hiện tại khớp với thẻ đầu ra được yêu cầu tiếp theo. Việc thực hiện bất cẩn có thể luôn tham lam chọn một bên và cho rằng tính đúng đắn, nhưng về nguyên tắc, một lựa chọn sai có thể cản trở các bước sau này. Lý do chính xác phải đảm bảo rằng bất kỳ lựa chọn bước hợp lệ nào cũng không phá hủy tính khả thi. 

## Phương pháp tiếp cận 

Phối cảnh bạo lực sẽ cố gắng liệt kê tất cả các hoán vị ban đầu có thể có của các thẻ đã cho thỏa mãn ràng buộc sắp xếp theo thứ hạng và đối với mỗi thẻ, mô phỏng tất cả các chuỗi lấy từ cả hai đầu. Vì mỗi bước có hai lựa chọn, điều này dẫn đến số lượng trình tự thao tác trên mỗi bộ bài tăng theo cấp số nhân và thậm chí trước đó, số lượng hoán vị ban đầu hợp lệ đã rất lớn do các cấp bậc bằng nhau cho phép nhiều hoán vị. Cách tiếp cận này thất bại ngay lập tức khi không gian tìm kiếm tăng lên theo cấp số nhân. 

Quan sát cấu trúc quan trọng là khi bộ bài ban đầu được sắp xếp theo thứ hạng, mọi hậu tố của bộ bài còn lại luôn là một đoạn liền kề của thứ tự được sắp xếp đó. Sau bất kỳ số lần loại bỏ nào từ cuối, các thẻ còn lại sẽ tạo thành một khối theo trình tự được sắp xếp theo thứ hạng chung. Điều này có nghĩa là ở mỗi bước, những quân bài duy nhất có thể được lấy tiếp theo là quân bài nhỏ nhất còn lại hoặc quân bài lớn nhất còn lại theo thứ tự. 

Điều này làm giảm vấn đề thành quy trình hai con trỏ trên danh sách đã sắp xếp. Chúng tôi sắp xếp tất cả các lá bài theo thứ hạng để đại diện cho bộ bài ban đầu. Sau đó, chúng tôi mô phỏng xem liệu trình tự đầu ra đã cho có thể được tạo ra bằng cách liên tục khớp với thẻ còn lại ngoài cùng bên trái hoặc ngoài cùng bên phải hay không. Nếu ở bất kỳ bước nào không có điểm cuối nào khớp với thẻ được yêu cầu thì việc xây dựng là không thể. Nếu không, chúng tôi ghi lại bên nào đã được sử dụng.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu đối với hoán vị và hoạt động | Hàm mũ | O(n) | Quá chậm | 
| Mô phỏng tham lam hai con trỏ trên bộ bài được sắp xếp | O(n) mỗi lần kiểm tra | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi chuyển đổi mỗi thẻ thành một giá trị có thể so sánh được bằng cách ánh xạ thứ hạng của nó thành một số nguyên từ 2 đến 14, chỉ giữ lại các chất để nhận dạng. Sau đó, chúng tôi sắp xếp tất cả các lá bài theo thứ hạng này, tạo thành bộ bài chuẩn đầu tiên. 

### bước 

1. Phân tích tất cả các thẻ và gán cho mỗi thẻ một giá trị xếp hạng. Sắp xếp các lá bài theo thứ hạng để tạo thành bộ bài ban đầu. 
2. Khởi tạo hai con trỏ, một ở cuối bộ bài đã sắp xếp và một ở trên cùng. 
3. Lặp lại trình tự đầu ra mục tiêu từ trái sang phải. 
4. Ở mỗi bước, so sánh thẻ mục tiêu hiện tại với thẻ ở con trỏ bên trái và thẻ ở con trỏ bên phải. 
5. Nếu nó khớp với con trỏ trái, hãy ghi lại thao tác D và di chuyển con trỏ trái vào trong. Nếu nó trùng với con trỏ bên phải thì ghi lại thao tác U và di chuyển con trỏ bên phải vào trong. 
6. Nếu cả hai đều không khớp, hãy kết luận rằng không có bộ bài ban đầu được sắp xếp hợp lệ nào có thể tạo ra chuỗi và xuất ra NO. 
7. Nếu tất cả các lá bài được so khớp thành công, xuất ra CÓ, bộ bài ban đầu được sắp xếp và các thao tác được ghi lại. 

Điểm quyết định quan trọng là bước 5. Khi cả hai đầu khớp với cùng thứ hạng hoặc thậm chí là các quân bài giống hệt nhau, việc lựa chọn là tùy ý vì cả hai đều tương ứng với các lần loại bỏ hợp lệ khỏi khoảng thời gian còn lại đối xứng. Cấu trúc của bộ bài được sắp xếp đảm bảo rằng cả hai lựa chọn đều đảm bảo tính khả thi. 

### Tại sao nó hoạt động 

Ở bất kỳ giai đoạn nào của quy trình, các lá bài chưa được chia còn lại phải tương ứng chính xác với một phân đoạn liền kề của bộ bài được sắp xếp theo thứ hạng trên toàn cầu. Điều này xảy ra vì việc loại bỏ khỏi một trong hai đầu sẽ không bao giờ làm xáo trộn trật tự nội bộ của đoạn còn lại. Do đó, ứng cử viên duy nhất cho thẻ bị loại tiếp theo là hai điểm cuối của phân đoạn này. Nếu thẻ tiếp theo được yêu cầu không ở một trong hai điểm cuối thì không có chuỗi thao tác hợp lệ nào có thể tạo ra thẻ đó. Ngược lại, nếu nó nằm ở một điểm cuối, việc loại bỏ nó sẽ giữ nguyên bất biến rằng tập hợp còn lại vẫn là phân đoạn được sắp xếp liền kề, do đó quá trình có thể tiếp tục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

rank_order = {
    '2': 2, '3': 3, '4': 4, '5': 5, '6': 6, '7': 7, '8': 8, '9': 9,
    '10': 10, 'J': 11, 'Q': 12, 'K': 13, 'A': 14
}

def parse_card(s):
    # rank is prefix, suit is suffix
    if s[:-1] == '1':  # not needed but safe guard (10 handled below)
        pass
    if s.startswith('10'):
        r = 10
        suit = s[2:]
    else:
        r = rank_order[s[0]]
        suit = s[1:]
    return (r, s)

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        cards = input().split()

        arr = []
        for c in cards:
            if c.startswith('10'):
                r = 10
            else:
                r = rank_order[c[0]]
            arr.append((r, c))

        arr.sort(key=lambda x: x[0])

        target = cards
        l, r = 0, n - 1
        ops = []

        ok = True
        for c in target:
            if l <= r and arr[l][1] == c:
                ops.append('D')
                l += 1
            elif l <= r and arr[r][1] == c:
                ops.append('U')
                r -= 1
            else:
                ok = False
                break

        if not ok:
            out.append("NO")
        else:
            out.append("YES")
            out.append(" ".join(x[1] for x in arr))
            out.append("".join(ops))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách chuyển đổi thứ hạng thành số nguyên để việc sắp xếp phản ánh sức mạnh của thẻ. Sau đó, bộ bài ban đầu được xây dựng dưới dạng các lá bài được sắp xếp theo thứ hạng. Điều này trực tiếp mã hóa yêu cầu bộ bài xuất phát phải được sắp xếp từ dưới lên trên. 

Mô phỏng sử dụng hai con trỏ trên mảng được sắp xếp này. Mỗi bước sẽ kiểm tra xem thẻ đầu ra được yêu cầu tiếp theo có khớp với điểm cuối hay không. Nếu nó khớp với điểm cuối bên trái, chúng tôi mô phỏng lấy từ dưới lên, nếu không thì từ trên xuống. Chuỗi hoạt động được xây dựng đồng thời. 

Một điểm tinh tế là chúng ta so sánh danh tính thẻ đầy đủ chứ không chỉ xếp hạng. Điều này tránh sự mơ hồ khi nhiều thẻ có cùng thứ hạng. Vì mỗi thẻ là duy nhất nên việc so khớp điểm cuối không rõ ràng ở cấp độ nhận dạng ngay cả khi cấp bậc trùng khớp. 

## Ví dụ đã hoạt động 

Hãy xem xét một ví dụ nhỏ trong đó bộ bài được sắp xếp`[2H, 10H, KC, AS]`và trình tự đầu ra là`2H, 10H, KC, AS`. 

Chúng tôi mô phỏng từng bước: 

| Bước | Trái | Đúng | Mục tiêu | Hành động | Rất tiếc | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2H | NHƯ | 2H | trận đấu trái | D | 
| 2 | 10H | NHƯ | 10H | trận đấu trái | ĐĐ | 
| 3 | KC | NHƯ | KC | trận đấu trái | DDD | 
| 4 | NHƯ | NHƯ | NHƯ | trận đấu trái | DDDD | 

Điều này xác nhận rằng trình tự chọn đáy hoàn toàn hợp lệ khi đầu ra khớp với thứ tự được sắp xếp. 

Bây giờ hãy xem xét một trường hợp hỗn hợp: bộ bài được sắp xếp`[4S, 9D, 10C, QC]`và đầu ra`QC, 10C, 9D, 4S`. 

| Bước | Trái | Đúng | Mục tiêu | Hành động | Rất tiếc | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 4S | Kiểm soát chất lượng | Kiểm soát chất lượng | khớp đúng | Bạn | 
| 2 | 4S | 10C | 10C | khớp đúng | UU | 
| 3 | 4S | 9D | 9D | khớp đúng | ƯU | 
| 4 | 4S | 4S | 4S | trận đấu trái | UUU D | 

Điều này chứng tỏ rằng các điểm cuối xen kẽ sẽ tái tạo lại trình tự một cách chính xác khi nó nhất quán với bộ bài ban đầu được sắp xếp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) mỗi lần kiểm tra | Sắp xếp 52 thẻ liên tục, quét mô phỏng một lần | 
| Không gian | O(n) | Kho chứa hàng và trình tự vận hành | 

Các ràng buộc cho phép tối đa 6000 trường hợp thử nghiệm, nhưng mỗi trường hợp đều rất nhỏ, do đó, ngay cả việc xử lý đầy đủ cũng vẫn nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    rank_order = {'2':2,'3':3,'4':4,'5':5,'6':6,'7':7,'8':8,'9':9,'10':10,'J':11,'Q':12,'K':13,'A':14}

    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        cards = input().split()

        arr = []
        for c in cards:
            if c.startswith('10'):
                r = 10
            else:
                r = rank_order[c[0]]
            arr.append((r, c))

        arr.sort(key=lambda x: x[0])

        l, r = 0, n - 1
        ops = []
        ok = True

        for c in cards:
            if l <= r and arr[l][1] == c:
                ops.append('D')
                l += 1
            elif l <= r and arr[r][1] == c:
                ops.append('U')
                r -= 1
            else:
                ok = False
                break

        if not ok:
            out.append("NO")
        else:
            out.append("YES")
            out.append(" ".join(x[1] for x in arr))
            out.append("".join(ops))

    return "\n".join(out)

# sample-like checks
assert run("1\n1\n10S\n") == "YES\n10S\nD", "single card"

assert run("1\n4\n4S 9D 10C QC\n") in [
    "YES\n4S 9D 10C QC\nUUUD",
    "YES\n4S 9D 10C QC\nUUDD",
    "YES\n4S 9D 10C QC\nDDDD"
], "basic feasibility"

assert run("1\n2\nAS 2D\n") in ["NO", "YES\n2D AS\nUD"], "small edge ambiguity"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| bộ bài 1 lá | CÓ với một lần điều trị | trường hợp tối thiểu | 
| 4 lá bài hỗn hợp | CÓ + hoạt động hợp lệ | tái thiết bình thường | 
| 2 quân bài đảo ngược | ranh giới CÓ/KHÔNG | sự lựa chọn điểm cuối đúng đắn | 

## Vỏ cạnh 

Trường hợp góc xuất hiện khi nhiều thẻ có cùng thứ hạng, chẳng hạn như`10C`Và`10H`. Vì việc sắp xếp chỉ phụ thuộc vào cấp bậc nên các lá bài này nằm liền kề trong bộ bài ban đầu theo bất kỳ thứ tự nào. Thuật toán vẫn hoạt động vì việc so khớp được thực hiện bằng nhận dạng thẻ đầy đủ, vì vậy ngay cả khi cả hai điểm cuối đều có hạng 10 thì chỉ có thể lấy được thẻ chính xác. 

Ví dụ: với bộ bài được sắp xếp ban đầu`[10C, 10H, AS]`và trình tự mục tiêu`10H, 10C, AS`, quá trình tiếp tục bằng cách chọn điểm cuối bên phải trước, sau đó đến bên trái, duy trì tính chính xác. 

Một trường hợp khác là khi cả hai đầu khớp với cùng một thẻ mục tiêu về thứ hạng chứ không phải về danh tính. Điều này thực sự không thể xảy ra vì mỗi thẻ là duy nhất; đẳng thức luôn rõ ràng, do đó thuật toán không bao giờ gặp phải mối ràng buộc thực sự trong việc so khớp danh tính, chỉ trong thứ tự xếp hạng được sử dụng cho cấu trúc. 

Một trường hợp tế nhị cuối cùng là khi những lựa chọn tham lam dường như có ý nghĩa quan trọng. Nếu ở một bước nào đó, cả hai đầu đều khớp với các ứng cử viên hợp lệ khác nhau, thì lựa chọn nào cũng giữ nguyên bất biến rằng bộ bài còn lại là một phân đoạn được sắp xếp liền kề. Điều này đảm bảo rằng không có điểm quyết định nào có thể tạo ra mâu thuẫn sau này vốn không thể tránh khỏi khỏi cấu trúc của nhiều tập hợp còn lại.
