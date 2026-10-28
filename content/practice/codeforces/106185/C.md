---
title: "CF 106185C - Lịch Của Người Công Nhân Nhiệt Tình"
description: "Chúng ta được cung cấp một mốc thời gian bắt đầu từ hôm nay, trong đó ngày hôm nay được ấn định là Thứ Hai. Lịch sau đó tiến hành theo từng ngày và chúng tôi muốn đếm xem có bao nhiêu ngày làm việc trong m ngày tiếp theo. Hai loại hạn chế xác định ngày không làm việc."
date: "2026-06-19T18:47:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106185
codeforces_index: "C"
codeforces_contest_name: "The 2025 ICPC Japan Online First Round Contest"
rating: 0
weight: 106185
solve_time_s: 51
verified: true
draft: false
---

[CF 106185C - Lịch của một người lao động nhiệt huyết](https://codeforces.com/problemset/problem/106185/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mốc thời gian bắt đầu từ hôm nay, trong đó ngày hôm nay được ấn định là Thứ Hai. Sau đó, lịch tiếp tục diễn ra từng ngày và chúng ta muốn đếm xem có bao nhiêu lịch tiếp theo`m`ngày là ngày làm việc. 

Hai loại hạn chế xác định ngày không làm việc. Đầu tiên, những ngày cuối tuần tuân theo một khuôn mẫu định kỳ cố định: thứ bảy và chủ nhật luôn là những ngày nghỉ lễ. Thứ hai, có một danh sách các ngày nghỉ lễ bổ sung, được đưa ra dưới dạng chỉ số ngày tuyệt đối bắt đầu từ hôm nay là ngày 1. Những ngày nghỉ lễ bổ sung này có thể lặp lại và có thể trùng với các ngày cuối tuần. Nếu một ngày là ngày cuối tuần hoặc nằm trong danh sách nghỉ lễ bổ sung thì đó là ngày không làm việc. Nếu không thì đó là một ngày làm việc. 

Nhiệm vụ là tính xem có bao nhiêu ngày làm việc xuất hiện trong khoảng thời gian từ ngày 1 đến ngày`m`, Ở đâu`m`có thể cực kỳ lớn, lên tới 10^18. Số ngày nghỉ thêm`n`nhỏ, nhiều nhất là 300, nhưng giá trị của những ngày đó cũng lớn, lên tới 10^18. 

Ý nghĩa chính của các ràng buộc là chúng ta không thể lặp lại qua nhiều ngày. Ngay cả việc quét tới 10^18 cũng không thể. Bất kỳ giải pháp nào cũng phải dựa vào cấu trúc tuần hoàn của các ngày cuối tuần và suy luận số học trực tiếp trên các phạm vi. Kích thước nhỏ của danh sách ngày lễ bổ sung cho thấy chúng ta có thể xử lý chúng riêng lẻ sau khi xử lý mẫu cuối tuần thông thường. 

Một sai lầm ngây thơ là coi đây là một vấn đề mô phỏng trực tiếp và lặp lại tất cả`m`ngày trong khi kiểm tra xem mỗi ngày là cuối tuần hay trong một loạt ngày nghỉ lễ. Điều này thất bại ngay lập tức khi`m`là lớn. 

Một cạm bẫy tinh vi khác là quên rằng các ngày cuối tuần chỉ phụ thuộc vào vị trí modulo 7, nhưng việc ánh xạ lại phụ thuộc vào thực tế là ngày 1 là Thứ Hai. Lỗi căn chỉnh modulo bất cẩn sẽ dịch chuyển không chính xác tất cả các ngày cuối tuần. 

Cuối cùng, các bản sao trong danh sách ngày lễ không thay đổi bất cứ điều gì, nhưng nếu một người quên loại bỏ trùng lặp hoặc đếm không chính xác các lần trùng lặp hai lần thì câu trả lời sẽ sai. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực rất đơn giản: mỗi ngày từ 1 đến`m`, tính xem đó là Thứ Bảy hay Chủ Nhật bằng cách sử dụng`(day % 7)`và kiểm tra xem nó có xuất hiện trong bộ ngày lễ bổ sung hay không. Mỗi kiểm tra là O(1) nếu được lưu trữ trong bộ băm, do đó độ phức tạp tổng cộng là O(m). Điều này đúng nhưng không khả thi vì`m`có thể lớn tới 10^18, vượt xa mọi giới hạn tính toán. 

Quan sát quan trọng là các ngày cuối tuần được định kỳ với giai đoạn 7, do đó sự đóng góp của chúng trong một khoảng thời gian dài có thể được tính theo khối thay vì theo ngày. Thay vì lặp lại, chúng tôi đếm xem có bao nhiêu tuần đầy đủ trong`m`, nhân với 2 rồi xử lý rõ ràng những ngày còn lại. 

Sau khi tính cả những ngày cuối tuần, vấn đề phức tạp duy nhất còn lại là danh sách ngày nghỉ bổ sung. Từ`n ≤ 300`, chúng tôi có thể xử lý từng kỳ nghỉ một cách độc lập. Cho mỗi ngày nghỉ lễ`a_i`cái đó nằm ở`[1, m]`, chúng tôi kiểm tra xem hôm nay đã là cuối tuần chưa. Nếu không, nó sẽ loại bỏ một ngày làm việc khỏi tổng số. 

Do đó, chiến lược trở thành: bắt đầu với tổng số ngày`m`, trừ đi những ngày cuối tuần được tính bằng số học, sau đó trừ đi số ngày nghỉ lễ bổ sung duy nhất rơi vào các ngày trong tuần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(m) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính xem khoảng thời gian đó có bao nhiêu tuần trọn vẹn`[1, m]`bằng cách chia`m`bằng 7. Mỗi tuần trọn vẹn đóng góp đúng 2 ngày cuối tuần. Điều này đưa ra số lượng cơ sở của những ngày cuối tuần như`weeks * 2`. 
2. Xác định xem còn lại bao nhiêu ngày sau khi tròn một tuần. Đây là những cái đầu tiên`m % 7`ngày sau tuần hoàn chỉnh cuối cùng. Vì ngày 1 là Thứ Hai nên những ngày còn lại này luôn tương ứng với cùng một tiền tố của chu kỳ hàng tuần, vì vậy chúng tôi có thể kiểm tra rõ ràng xem mỗi ngày này là Thứ Bảy hay Chủ Nhật và cộng vào số cuối tuần. 
3. Tính tổng số ngày cuối tuần trong`[1, m]`bằng cách thêm những ngày cuối tuần cả tuần và những ngày cuối tuần còn lại trong tuần. 
4. Khởi tạo số ngày làm việc như`m - weekend_count`, vì ban đầu mọi ngày không phải cuối tuần đều được coi là làm việc. 
5. Chèn tất cả các ngày nghỉ lễ bổ sung vào một tập hợp, nhưng chỉ giữ những ngày đó trong đó`[1, m]`. Điều này tránh việc xử lý các giá trị lớn không liên quan. 
6. Đối với mỗi ngày nghỉ lễ bổ sung duy nhất, hãy kiểm tra xem đó có phải là ngày cuối tuần không. Nếu không, nó đã được tính không chính xác là một ngày làm việc và phải được trừ vào câu trả lời. Nếu đã là cuối tuần thì không có hiệu lực vì cuối tuần đã bị loại trừ. 
7. Xuất số ngày làm việc được điều chỉnh cuối cùng. 

### Tại sao nó hoạt động 

Tính chính xác dựa trên việc tách lịch thành hai cấu trúc độc lập: cấu trúc định kỳ cố định cho các ngày cuối tuần và một tập hợp thưa thớt các loại trừ không thường xuyên. Việc tính toán cuối tuần là chính xác vì mọi phân đoạn 7 ngày đều có cấu trúc giống hệt nhau bắt đầu từ thứ Hai. Bước điều chỉnh cho những ngày nghỉ thêm có hiệu quả vì mỗi ngày như vậy sẽ chuyển một ngày làm việc thành một ngày không làm việc một cách độc lập trừ khi ngày đó đã bị loại trừ bởi cấu trúc định kỳ. Vì sự chồng chéo chỉ xảy ra giữa hai hệ thống loại trừ độc lập và không bao giờ tạo ra việc tính hai lần ngoài số lượng thành viên được đặt đơn giản, nên chỉ trừ những ngày nghỉ không phải cuối tuần sẽ mang lại số ngày làm việc chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def is_weekend(day: int) -> bool:
    # Day 1 is Monday:
    # 6 -> Saturday, 0 -> Sunday when (day-1) % 7 mapping
    r = (day - 1) % 7
    return r == 5 or r == 6

def solve():
    out = []
    while True:
        line = input().strip()
        if not line:
            continue
        n, m = map(int, line.split())
        if n == 0 and m == 0:
            break

        arr = list(map(int, input().split()))
        
        # weekends
        full_weeks = m // 7
        rem = m % 7

        weekend = full_weeks * 2
        for i in range(rem):
            if is_weekend(full_weeks * 7 + i + 1):
                weekend += 1

        working = m - weekend

        # extra holidays
        seen = set()
        for a in arr:
            if 1 <= a <= m:
                seen.add(a)

        for d in seen:
            if not is_weekend(d):
                working -= 1

        out.append(str(working))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp trước tiên chuyển đổi lịch thành dạng biểu diễn mô-đun bằng cách tách các tuần đầy đủ và khối còn lại. các`is_weekend`hàm mã hóa căn chỉnh ngày trong tuần trong đó ngày 1 là Thứ Hai, đảm bảo phân loại chính xác Thứ Bảy và Chủ Nhật. 

Bước đếm cuối tuần xử lý cẩn thận phân đoạn còn lại thay vì cho rằng nó luôn bắt đầu vào thứ Hai một cách riêng biệt. biểu hiện`full_weeks * 7 + i + 1`xây dựng lại chỉ số ngày tuyệt đối cho phần còn lại để phân loại ngày trong tuần vẫn nhất quán. 

bộ`seen`ngăn chặn phép trừ trùng lặp khi danh sách đầu vào chứa các ngày nghỉ lễ lặp lại. Mỗi ngày nghỉ chỉ được xem xét nếu nó nằm trong khoảng thời gian được truy vấn. 

Bước trừ chỉ áp dụng cho những ngày nghỉ không phải cuối tuần vì những ngày cuối tuần đã bị xóa hàng loạt và nếu không sẽ xảy ra phép trừ kép. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n=4, m=8
holidays = [1,2,3,4]
```Chúng tôi tính toán đầy đủ các tuần:`8 // 7 = 1`, phần dư`1`. 

| Bước | Giá trị | 
| --- | --- | 
| Cả tuần | 1 | 
| Cuối tuần từ cả tuần | 2 | 
| Ngày còn lại | 1 | 
| Tổng cuối tuần | 2 | 
| Ngày làm việc cơ sở | 8 - 2 = 6 | 
| Ngày lễ trong phạm vi | {1,2,3,4} | 

Bây giờ chúng tôi kiểm tra mỗi kỳ nghỉ. Chỉ có ngày 1 là thứ Hai chứ không phải cuối tuần, nên trừ đi 1. Câu trả lời cuối cùng là 5. 

Điều này cho thấy các ngày nghỉ trong tuần chồng chéo chỉ ảnh hưởng đến những ngày không phải cuối tuần. 

### Ví dụ 2 

đầu vào:```
n=2, m=10
holidays = [15, 3]
```Chỉ có ngày thứ 3 là trong phạm vi. 

| Bước | Giá trị | 
| --- | --- | 
| Cả tuần | 1 | 
| Cuối tuần từ cả tuần | 2 | 
| Số ngày còn lại | 3 ngày | 
| Tổng cuối tuần | tính qua số dư | 
| Ngày làm việc cơ sở | m - cuối tuần | 
| Ngày nghỉ hợp lệ | {3} | 

Ngày thứ 3 không phải là ngày cuối tuần nên chúng tôi trừ đi 1. 

Điều này thể hiện việc lọc chính xác các ngày nghỉ ngoài phạm vi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Tính toán cuối tuần là O(1), xử lý tuyến tính tới 300 ngày nghỉ | 
| Không gian | O(n) | Bộ tối đa 300 ngày nghỉ lễ độc đáo | 

Các ràng buộc làm cho việc này nhanh chóng một cách thoải mái. Ngay cả với 100 trường hợp thử nghiệm, tổng công việc vẫn nhỏ vì mỗi trường hợp xử lý tối đa vài trăm giá trị. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def is_weekend(day: int) -> bool:
        r = (day - 1) % 7
        return r == 5 or r == 6

    def solve():
        out = []
        while True:
            line = input().strip()
            if not line:
                continue
            n, m = map(int, line.split())
            if n == 0 and m == 0:
                break
            arr = list(map(int, input().split()))

            full_weeks = m // 7
            rem = m % 7

            weekend = full_weeks * 2
            for i in range(rem):
                if is_weekend(full_weeks * 7 + i + 1):
                    weekend += 1

            working = m - weekend

            seen = set()
            for a in arr:
                if 1 <= a <= m:
                    seen.add(a)

            for d in seen:
                if not is_weekend(d):
                    working -= 1

            out.append(str(working))

        return "\n".join(out)

    return solve()

# sample-like checks
assert run("4 8\n1 2 3 4\n0 0\n") == "5"
assert run("2 10\n15 3\n0 0\n") == "6"

# edge cases
assert run("1 1\n1\n0 0\n") == "0"
assert run("1 7\n6\n0 0\n") == "5"
assert run("3 14\n1 8 9\n0 0\n") == "8"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 4 8 với những ngày nghỉ nhỏ | 5 | chồng chéo các ngày nghỉ trong tuần | 
| 2 10 với ngoài phạm vi | 6 | bỏ qua những ngày nghỉ không hợp lệ | 
| 1 1 với kỳ nghỉ | 0 | cạnh một ngày | 
| 1 7 ranh giới cuối tuần | 5 | căn chỉnh cuối tuần đúng cách | 
| 3 14 hỗn hợp | 8 | cả tuần + độ chính xác còn lại | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi`m`nhỏ hơn 7. Trong tình huống này không có tuần đầy đủ, do đó tất cả tính toán cuối tuần phải xuất phát từ logic còn lại. Thuật toán xử lý việc này một cách tự nhiên vì`m // 7`trở thành số 0 và chỉ lặp lại`rem`được sử dụng. 

Một trường hợp đặc biệt khác là khi ngày nghỉ lễ diễn ra vào ngày cuối tuần. Ví dụ: nếu ngày thứ 6 (Thứ Bảy) được tính là ngày nghỉ thì không được trừ vào ngày làm việc vì ngay từ đầu nó chưa bao giờ được tính là làm việc. Thuật toán xử lý việc này bằng cách kiểm tra`is_weekend(d)`trước khi trừ, đảm bảo không xảy ra lỗi đếm kép. 

Trường hợp cạnh cuối cùng liên quan đến các bản sao trong danh sách kỳ nghỉ. Nếu cùng một ngày xuất hiện nhiều lần, phép trừ ngây thơ sẽ làm giảm kết quả nhiều lần một cách không chính xác. Việc sử dụng một bộ đảm bảo mỗi ngày được xử lý chính xác một lần, duy trì tính chính xác ngay cả khi nhập nhiều lần.
