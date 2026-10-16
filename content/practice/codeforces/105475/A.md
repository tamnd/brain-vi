---
title: "CF 105475A - Pizza"
description: "Mỗi người tham dự yêu cầu một số lát bánh pizza và mỗi lát được yêu cầu đều có lớp phủ cụ thể. Một cửa hàng pizza bán pizza chỉ có một lớp phủ trên mỗi chiếc bánh pizza và mỗi chiếc bánh pizza luôn được cắt thành đúng 8 lát bằng nhau."
date: "2026-06-23T02:12:03+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105475
codeforces_index: "A"
codeforces_contest_name: "XXII Spain Olympiad in Informatics, Day 1"
rating: 0
weight: 105475
solve_time_s: 93
verified: true
draft: false
---

[CF 105475A - Pizza](https://codeforces.com/problemset/problem/105475/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 33s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi người tham dự yêu cầu một số lát bánh pizza và mỗi lát được yêu cầu đều có lớp phủ cụ thể. Một cửa hàng pizza bán pizza chỉ có một lớp phủ trên mỗi chiếc bánh pizza và mỗi chiếc bánh pizza luôn được cắt thành đúng 8 lát bằng nhau. 

Nhiệm vụ là quyết định xem có thể mua một số lượng pizza cho mỗi lớp phủ sao cho mỗi lát được yêu cầu đều được đáp ứng chính xác và không còn miếng nào chưa được sử dụng. Một hạn chế chính là không được phép sử dụng các lát còn sót lại, vì vậy mỗi lát đã mua phải được ai đó tiêu thụ. 

Điều này biến vấn đề thành một câu hỏi về việc liệu nhu cầu về mỗi loại lớp phủ có thể được gói chính xác thành các nhóm cỡ 8 hay không, vì mỗi chiếc bánh pizza đóng góp chính xác 8 lát giống hệt nhau của một lớp phủ. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Đối với mỗi trường hợp, chúng tôi được cung cấp số lượng người và số lượng lớp phủ có sẵn. Sau đó, với mỗi người, chúng tôi được cung cấp số lượng lát họ muốn, theo sau là danh sách các loại đồ ăn kèm cho những lát đó. Đầu ra của mỗi trường hợp là một quyết định: liệu có tồn tại một kế hoạch mua hàng hoàn hảo hay không. 

Các ràng buộc cho phép tối đa 1000 người trong mỗi trường hợp thử nghiệm và tổng cộng 100000 yêu cầu lát cho mỗi trường hợp. Điều này có nghĩa là chúng ta cần một giải pháp tuyến tính hoặc gần tuyến tính trong tổng số yêu cầu lát cắt. Bất cứ điều gì bậc hai trên người hoặc lát cắt sẽ quá chậm. 

Một trường hợp khó nhận thấy là khi việc phân phối có vẻ “cân bằng cho mỗi người” nhưng không chia hết cho mỗi người trên toàn cầu. Ví dụ: nếu một người yêu cầu 3 lát topping 0 và người khác yêu cầu 5 lát topping 0, thì mọi thứ có vẻ ổn cục bộ, nhưng tổng số là 8 nên nó hoạt động. Tuy nhiên, nếu tổng số là 10 và 6 ở những người khác nhau, việc kiểm tra đơn giản cho mỗi người có thể chấp nhận hoặc từ chối không chính xác tùy thuộc vào việc thực hiện, mặc dù quyết định đúng chỉ phụ thuộc vào tổng số tiền cho mỗi lần thêm. 

Một trường hợp khác phát sinh khi lớp phủ thưa thớt ở đầu vào. Một giải pháp ngây thơ có thể bỏ qua các lớp phủ không có yêu cầu nào, nhưng những thứ đó không liên quan vì chúng không ảnh hưởng đến tính khả thi. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là suy nghĩ về mặt nhiệm vụ. Mỗi yêu cầu cắt lát phải được chỉ định cho một chiếc bánh pizza và mỗi chiếc bánh pizza đóng góp chính xác 8 lát giống hệt nhau. Người ta có thể thử mô phỏng việc nhóm các lát thành bộ 8 miếng trên mỗi lớp trên, nhưng điều đó nhanh chóng trở thành việc ghi sổ không cần thiết. 

Quan sát quan trọng là pizza có lớp phủ độc lập với nhau. Một chiếc bánh pizza có topping t chỉ có thể phục vụ những lát topping t và nó luôn đóng góp chính xác 8 lát. Do đó, đối với mỗi phần topping độc lập, tổng số lát yêu cầu của phần topping đó phải chia hết cho 8. Nếu điều này đúng, chúng ta chỉ cần mua Total_sum[t] / 8 chiếc pizza có topping t. 

Điều này làm giảm toàn bộ vấn đề trong việc tính tổng cho mỗi lần phủ và kiểm tra điều kiện chia hết. Cấu trúc của đầu vào đảm bảo rằng việc đếm là đủ vì không có tương tác chéo và không có ràng buộc về thứ tự. 

Cách giải thích bạo lực sẽ cố gắng mô phỏng việc làm đầy từng miếng bánh pizza, điều này đòi hỏi phải duy trì nhiều bộ bánh pizza được làm đầy một phần trên mỗi lớp phủ. Trong trường hợp xấu nhất, với tối đa 100000 lát, mô phỏng đó sẽ vẫn tuyến tính trên mỗi lần chèn nhưng phức tạp và dễ xảy ra lỗi một cách không cần thiết, đặc biệt là khi xử lý phần lấp đầy một phần. 

Phương pháp dựa trên tổng được đơn giản hóa sẽ loại bỏ tất cả theo dõi trạng thái và thay thế nó bằng một tập hợp chuyển duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (mô phỏng nhân bánh pizza) | O(tổng số lát) nhưng có chi phí lớn | O(số lượng pizza một phần) | Quá phức tạp | 
| Tối ưu (tổng mỗi topping) | O(tổng số lát) | O(M) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một mảng`cnt`có kích thước M với các số 0 để lưu trữ tổng số lát được yêu cầu trên mỗi lớp phủ. Mảng này thể hiện số lát chúng ta phải cung cấp cho mỗi lớp phủ. 
2. Đối với mỗi người, hãy đọc danh sách các món ăn kèm được yêu cầu của họ và tăng dần các mục tương ứng trong`cnt`. Mỗi mục trong mảng này tích lũy nhu cầu toàn cầu chứ không phải cấu trúc mỗi người. 
3. Sau khi xử lý tất cả mọi người, lặp lại tất cả các phần trên từ 0 đến M − 1 và kiểm tra xem`cnt[t]`chia hết cho 8. Điều kiện này đảm bảo rằng nhu cầu có thể được phân chia chính xác thành những chiếc pizza đầy đủ với lớp phủ đó. 
4. Nếu bất kỳ phần topping nào có số dư khi chia cho 8, hãy kết luận ngay rằng không thể đáp ứng mọi yêu cầu mà không lãng phí. 
5. Nếu tất cả các loại lớp phủ đều thỏa mãn điều kiện chia hết, hãy kết luận rằng có một lựa chọn pizza hợp lệ. 

Bước lập luận quan trọng là mỗi chiếc bánh pizza đóng góp chính xác 8 lát giống hệt nhau, do đó tính khả thi sẽ giảm xuống khi phân chia tổng nhu cầu trên mỗi lớp phủ thành các nhóm cỡ 8. 

### Tại sao nó hoạt động 

Đối với mỗi lớp phủ, chúng tôi chỉ tương tác với các đơn vị giống hệt nhau, các lát có cùng hương vị. Bất kỳ giải pháp hợp lệ nào cũng phải chia các lát này thành các nhóm 8 miếng riêng biệt, vì mỗi nhóm tương ứng chính xác với một chiếc bánh pizza đã mua. Nếu tổng số không chia hết cho 8, thì ít nhất một lát sẽ không được nhóm lại hoặc buộc phải có một chiếc bánh pizza chưa hoàn chỉnh, điều này bị cấm. Ngược lại, nếu tính chia hết được giữ nguyên, thì việc xây dựng giải pháp sẽ đơn giản bằng cách lấy chính xác cnt[t] / 8 chiếc pizza của mỗi lớp phủ và gán các lát cắt của chúng một cách tùy ý để phù hợp với yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_case(n, m):
    cnt = [0] * m

    for _ in range(n):
        data = list(map(int, input().split()))
        p = data[0]
        toppings = data[1:]
        for t in toppings:
            cnt[t] += 1

    for x in cnt:
        if x % 8 != 0:
            return "NO"
    return "SI"

def main():
    out = []
    data = sys.stdin.read().strip().split()
    it = iter(data)

    while True:
        try:
            n = int(next(it))
            m = int(next(it))
        except StopIteration:
            break

        cnt = [0] * m
        for _ in range(n):
            p = int(next(it))
            for _ in range(p):
                t = int(next(it))
                cnt[t] += 1

        ok = True
        for x in cnt:
            if x % 8 != 0:
                ok = False
                break

        out.append("SI" if ok else "NO")

    print("\n".join(out))

if __name__ == "__main__":
    main()
```Giải pháp đọc đầu vào theo kiểu truyền phát vì tổng số số nguyên có thể lớn. Mỗi yêu cầu lát được xử lý chính xác một lần, tăng bộ đếm phần trên tương ứng. 

Điều tinh tế duy nhất là đảm bảo rằng tất cả mã thông báo được sử dụng chính xác trên nhiều trường hợp thử nghiệm. Việc sử dụng một trình lặp trên toàn bộ đầu vào sẽ tránh được chi phí I/O lặp lại và đơn giản hóa việc phân tích cú pháp. 

## Ví dụ đã hoạt động 

Chúng tôi sử dụng thông tin đầu vào mẫu được cung cấp để minh họa hành vi. 

### Ví dụ 1 

đầu vào:```
1 1
8 0 0 0 0 0 0 0 0
```| Bước | Đứng đầu số 0 | Kiểm tra | 
| --- | --- | --- | 
| Sau khi đọc người 1 | 8 | đang chờ xử lý | 

Kiểm tra lần cuối: 8 % 8 = 0, do đó đầu ra là SI. 

Điều này xác nhận rằng việc nhóm chính xác thành một chiếc bánh pizza sẽ có tác dụng. 

### Ví dụ 2 

đầu vào:```
3 3
3 0 1 2
3 2 2 0
3 1 1 1
```| Bước | cnt[0] | cnt[1] | cnt[2] | Kiểm tra | 
| --- | --- | --- | --- | --- | 
| Sau tất cả đầu vào | 2 | 4 | 2 | cuối cùng | 

Không có giá trị nào trong số này chia hết cho 8, vì vậy câu trả lời là KHÔNG. 

Điều này cho thấy trường hợp mặc dù nhu cầu được phân phối nhưng không có lớp phủ nào tạo thành một bộ 8 hoàn chỉnh. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(tổng số lát + M) | Mỗi lát được xử lý một lần, sau đó mỗi phần phủ được kiểm tra một lần | 
| Không gian | O(M) | Chỉ có mảng đếm trên mỗi phần trên được lưu trữ | 

Các ràng buộc cho phép tổng cộng tối đa 100000 yêu cầu lát cắt cho mỗi trường hợp thử nghiệm, do đó, việc chuyển tuyến tính qua tất cả các yêu cầu là đủ nhanh một cách dễ dàng. Việc sử dụng bộ nhớ vẫn ở mức nhỏ vì M tối đa là 100. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    data = sys.stdin.read().strip().split()
    it = iter(data)

    out = []
    while True:
        try:
            n = int(next(it))
            m = int(next(it))
        except StopIteration:
            break

        cnt = [0] * m
        for _ in range(n):
            p = int(next(it))
            for _ in range(p):
                cnt[int(next(it))] += 1

        out.append("SI" if all(x % 8 == 0 for x in cnt) else "NO")

    return "\n".join(out)

# provided sample
assert run("""1 1
8 0 0 0 0 0 0 0 0
""") == "SI"

# sample 2
assert run("""3 3
3 0 1 2
3 2 2 0
3 1 1 1
""") == "NO"

# all zeros but valid multiples of 8
assert run("""1 2
8 0 0 0 0 0 0 0 0
""") == "SI"

# impossible single topping
assert run("""1 1
1 0
""") == "NO"

# multiple test cases
assert run("""1 1
8 0 0 0 0 0 0 0 0
2 1
4 0 0 0 0
4 0 0 0 0
""") == "SI
NO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| một chiếc bánh pizza đầy đủ hợp lệ | SI | tính đúng đắn cơ bản | 
| lớp phủ hỗn hợp không hợp lệ | KHÔNG | logic tổng hợp | 
| nhiều trường hợp thử nghiệm | SI KHÔNG | phân tích nhiều chữ | 
| trường hợp nhỏ không thể | KHÔNG | lỗi chia hết | 

## Vỏ cạnh 

Một trường hợp có một phần trên cùng và một yêu cầu một lát sẽ hiển thị trực tiếp quy tắc chia hết. Nếu đầu vào là`1 1`theo sau là`1 0`, bộ đếm cho đỉnh 0 trở thành 1, và vì 1 % 8 khác 0 nên thuật toán sẽ loại bỏ nó ngay lập tức. 

Trường hợp tất cả các nhu cầu chính xác là bội số của 8 trên mỗi lớp phủ xác nhận kịch bản tích cực. Nếu topping 0 có 16 yêu cầu và topping 1 có 8 yêu cầu thì cả hai đều chia hết cho 8 nên thuật toán chấp nhận, khớp với thực tế là chúng ta có thể mua được 2 chiếc pizza topping 0 và 1 pizza topping 1. 

Một trường hợp có nhiều người nhưng yêu cầu mỗi người nhỏ xác nhận rằng cấu trúc mỗi người là không phù hợp. Ngay cả khi mỗi người yêu cầu kết hợp các lớp phủ, chỉ tổng số cuối cùng mới quan trọng và thuật toán tổng hợp chính xác mọi thứ trước khi kiểm tra khả năng chia hết.
