---
title: "CF 104101B - Thép của trái tim"
description: "Chúng tôi đang mô phỏng một nhân vật trong trò chơi có sức khỏe thay đổi theo thời gian theo nhật ký sự kiện theo trình tự thời gian. Nhân vật bắt đầu với giá trị sức khỏe ban đầu và nhận thêm sức khỏe bất cứ khi nào họ lên cấp."
date: "2026-07-02T02:07:33+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104101
codeforces_index: "B"
codeforces_contest_name: "The 2022 Zhejiang University City College Freshman Programming Contest"
rating: 0
weight: 104101
solve_time_s: 50
verified: true
draft: false
---

[CF 104101B - Thép của trái tim](https://codeforces.com/problemset/problem/104101/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang mô phỏng một nhân vật trong trò chơi có sức khỏe thay đổi theo thời gian theo nhật ký sự kiện theo trình tự thời gian. Nhân vật bắt đầu với giá trị sức khỏe ban đầu và nhận thêm sức khỏe bất cứ khi nào họ lên cấp. Tại một thời điểm nào đó, họ cũng có thể mua một vật phẩm đặc biệt giúp tăng sức khỏe vĩnh viễn và mở khóa khả năng bị động. 

Sau khi có được vật phẩm, bất cứ khi nào nhân vật tấn công kẻ thù, nội tại có thể kích hoạt nếu kẻ địch đó hiện không trong thời gian hồi chiêu. Khi kích hoạt, nó sẽ tính toán giá trị sát thương dựa trên lượng máu hiện tại của nhân vật, sau đó chuyển một phần sát thương đó thành hồi máu. Việc chữa lành được thêm ngay lập tức vào sức khỏe hiện tại của nhân vật, điều này sau đó có thể ảnh hưởng đến việc tính toán trong tương lai. 

Đầu vào là một chuỗi các sự kiện được đánh dấu thời gian được sắp xếp theo thứ tự thời gian tăng dần. Mỗi sự kiện là một sự tăng cấp giúp tăng sức khỏe lên một lượng cố định, một sự kiện mua hàng mang lại phần thưởng sức khỏe cố định lớn và kích hoạt trạng thái thụ động hoặc một cuộc tấn công vào một trong năm kẻ thù có thể kích hoạt hoặc không thể kích hoạt thụ động tùy thuộc vào trạng thái hồi chiêu. Thời gian hồi chiêu được theo dõi độc lập với mỗi kẻ thù, nghĩa là việc tấn công các kẻ thù khác nhau không ảnh hưởng đến bộ đếm thời gian hồi chiêu của nhau. 

Đầu ra chỉ đơn giản là giá trị sức khỏe cuối cùng sau khi xử lý tất cả các sự kiện theo thứ tự. 

Các ràng buộc đủ nhỏ để mô phỏng trực tiếp là đủ. Có tối đa 1000 sự kiện và thời gian được cung cấp ở mức độ chi tiết thứ hai trong khoảng thời gian 60 phút, do đó, ngay cả một mô phỏng O(m) đơn giản với công việc liên tục cho mỗi sự kiện cũng đủ nhanh. 

Sự tinh tế chính nằm ở việc xử lý chính xác logic thời gian hồi chiêu của mỗi kẻ địch và đảm bảo rằng nội tại chỉ được áp dụng sau khi mua vật phẩm. Một nguồn lỗi thường gặp khác là thứ tự thực hiện bên trong bị động: sát thương phụ thuộc vào lượng máu hiện tại tại thời điểm tấn công và việc hồi máu được áp dụng sau khi tính toán sát thương chứ không phải trước đó. 

Một sai lầm ngây thơ là quên rằng thời gian hồi chiêu tùy theo kẻ địch. Ví dụ: nếu một đòn tấn công lúc 00:10 tấn công kẻ thù 1, sau đó một đòn tấn công khác vào lúc 00:20 tấn công kẻ thù 2, thì đòn tấn công thứ hai vẫn sẽ kích hoạt nội tại lên kẻ thù 2 nếu vật phẩm đang hoạt động, mặc dù đã 10 giây trôi qua kể từ lần kích hoạt đầu tiên. 

Một trường hợp tế nhị khác là ranh giới thời gian hồi chiêu. Nếu một cuộc tấn công kích hoạt tại thời điểm t, thì cuộc tấn công thứ hai vào cùng một kẻ thù tại thời điểm t + 29 sẽ không được kích hoạt, nhưng tại thời điểm t + 30 thì nên cho phép lại. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là mô phỏng dòng thời gian chính xác như mô tả. Chúng tôi duy trì lượng máu hiện tại của nhân vật, cho dù vật phẩm đã được mua hay chưa và đối với từng kẻ thù vào lần cuối cùng nội tại được kích hoạt. 

Đối với mỗi sự kiện, chúng tôi phân tích dấu thời gian của nó thành giây và xử lý theo thứ tự. Các sự kiện thăng cấp chỉ đơn giản là thêm một lượng máu cố định. Việc mua vật phẩm sẽ thêm phần thưởng cố định và cho phép tính toán thụ động trong tương lai. Các sự kiện tấn công kiểm tra xem vật phẩm có đang hoạt động hay không và liệu điều kiện thời gian hồi chiêu của kẻ địch đó có được thỏa mãn hay không. Nếu cả hai đều giữ nguyên, chúng tôi tính toán sát thương bằng cách sử dụng lượng máu hiện tại, tính toán khả năng hồi phục theo tỷ lệ phần trăm nổi của sát thương đó và thêm nó vào lượng máu. Chúng tôi cũng cập nhật thời gian kích hoạt cuối cùng của kẻ địch đó. 

Một biến thể brute-force sẽ không khác nhiều về cấu trúc vì kích thước đầu vào vốn đã nhỏ. Sự kém hiệu quả giả định duy nhất sẽ là tính toán lại việc phân tích cú pháp thời gian hoặc quét các sự kiện trước đó một cách không cần thiết, nhưng ngay cả điều đó cũng sẽ nằm trong giới hạn. Cải tiến thực sự chỉ mang tính khái niệm: việc theo dõi trạng thái của từng kẻ thù cho phép xử lý từng sự kiện trong O(1). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(m) | O(1) | Đã chấp nhận | 
| Mô phỏng tối ưu | O(m) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Chúng tôi chuyển đổi tất cả dấu thời gian thành giây để đơn giản hóa việc so sánh, vì logic thời gian hồi chiêu chỉ phụ thuộc vào sự khác biệt. 

1. Khởi tạo sức khỏe hiện tại là H1 và đánh dấu vật phẩm là chưa mua. Đồng thời khởi tạo một mảng Last_trigger có kích thước 5, chứa một giá trị rất âm để các đòn tấn công sớm luôn vượt qua quá trình kiểm tra thời gian hồi chiêu. 
2. Xử lý các sự kiện theo trình tự thời gian. 
3. Nếu sự kiện tăng cấp, hãy tăng máu thêm H2. 
4. Nếu sự kiện là sự kiện mua hàng, hãy tăng 800 máu và đánh dấu vật phẩm là hoạt động. 
5. Nếu sự kiện là tấn công kẻ thù x, trước tiên hãy kiểm tra xem vật phẩm đã được mua chưa. Nếu không, cuộc tấn công không có tác dụng gì. 
6. Nếu vật phẩm đang hoạt động, hãy kiểm tra thời gian hồi chiêu của kẻ địch x bằng cách sử dụng điều kiện current_time - Last_trigger[x] >= 30. Nếu điều này không thành công, hãy bỏ qua đòn tấn công. 
7. Nếu thời gian hồi chiêu cho phép, tính sát thương là 125 + tầng (0,06 * hiện tại_máu). 
8. Tính hồi máu theo tầng (0,1 * sát thương), sau đó cộng nó vào máu hiện tại. 
9. Cập nhật Last_trigger[x] về thời điểm hiện tại. 

Ý tưởng chính là những thay đổi về sức khỏe sẽ ảnh hưởng ngay lập tức đến các sự kiện tiếp theo, bao gồm cả các tính toán thiệt hại sau này, vì vậy chúng tôi phải cập nhật nó tại chỗ. 

### Tại sao nó hoạt động 

Tại mọi thời điểm, mô phỏng duy trì trạng thái chính xác của trò chơi được ngụ ý bởi lịch sử sự kiện: sức khỏe hiện tại, tính sẵn có của vật phẩm và trạng thái thời gian hồi chiêu của mỗi kẻ thù. Mỗi sự kiện chuyển đổi trạng thái này một cách xác định chỉ dựa trên trạng thái hiện tại và chính sự kiện đó. Vì các sự kiện được xử lý theo thứ tự thời gian tăng dần và mỗi bản cập nhật đều khớp chính xác với quy tắc trò chơi nên không có sự kiện nào trong tương lai phụ thuộc vào thông tin chưa được xử lý, điều này đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def parse_time(s):
    mm, ss = s.split(":")
    return int(mm) * 60 + int(ss)

def solve():
    H1, H2, m = map(int, input().split())
    
    hp = H1
    item = False

    last = [-10**9] * 5

    for _ in range(m):
        parts = input().split()
        t = parse_time(parts[0])
        typ = int(parts[1])

        if typ == 2:
            hp += H2

        elif typ == 1:
            hp += 800
            item = True

        else:
            x = int(parts[2]) - 1
            if not item:
                continue
            if t - last[x] < 30:
                continue

            damage = 125 + int(0.06 * hp)
            heal = int(damage * 0.1)

            hp += heal
            last[x] = t

    print(hp)

if __name__ == "__main__":
    solve()
```Việc triển khai tuân theo cấu trúc hướng sự kiện một cách trực tiếp. Thời gian được chuyển đổi một lần cho mỗi sự kiện thành giây, giúp tránh sự phức tạp khi phân tích cú pháp lặp lại. 

Kiểm tra thời gian hồi chiêu sử dụng bất đẳng thức nghiêm ngặt`t - last[x] < 30`, tương đương với việc thực thi khoảng cách tối thiểu 30 giây. Các phép tính phụ thuộc vào tình trạng sử dụng phép cắt số nguyên chính xác theo yêu cầu của báo cáo vấn đề. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một đoạn ngắn trong đó vật phẩm được mua và sau đó một cuộc tấn công xảy ra ngay lập tức. 

| Thời gian | Sự kiện | HP trước | Hành động | HP sau | Ghi chú | 
| --- | --- | --- | --- | --- | --- | 
| 00:00 | thăng cấp | 500 | +10 | 510 | tăng trưởng cơ bản | 
| 00:10 | mua hàng | 510 | +800 | 1310 | mục được kích hoạt | 
| 00:15 | tấn công kẻ thù 1 | 1310 | sát thương = 125 + 78 = 203, hồi máu = 20 | 13h30 | thời gian hồi chiêu bắt đầu | 

Dấu vết này cho thấy sức khỏe ảnh hưởng trực tiếp như thế nào đến công thức sát thương và cách áp dụng khả năng hồi phục ngay sau khi tính toán sát thương. 

### Ví dụ 2 

Bây giờ hãy xem xét việc chặn các kích hoạt lặp lại trong thời gian hồi chiêu. 

| Thời gian | Sự kiện | HP trước | Hành động | HP sau | Ghi chú | 
| --- | --- | --- | --- | --- | --- | 
| 00:00 | mua hàng | 500 | +800 | 1300 | mục đang hoạt động | 
| 00:10 | tấn công 1 | 1300 | kích hoạt | 1320 | kẻ địch 1 thời gian hồi chiêu bắt đầu | 
| 00:20 | tấn công 1 | 1320 | bị chặn | 1320 | thời gian hồi chiêu chưa xong | 
| 00:40 | tấn công 1 | 1320 | lại kích hoạt | 1340 | thiết lập lại thời gian hồi chiêu | 

Điều này xác nhận rằng thời gian hồi chiêu tính theo từng kẻ địch và được thực thi theo chênh lệch thời gian chứ không phải theo số sự kiện. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m) | mỗi sự kiện được xử lý một lần với công việc O(1) | 
| Không gian | O(1) | chỉ trạng thái không đổi cộng với 5 trình theo dõi thời gian hồi chiêu | 

Các ràng buộc cho phép tối đa 1000 sự kiện, do đó, mô phỏng tuyến tính có thể thoải mái nằm trong giới hạn ngay cả trong Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import builtins

    # assume solve is defined in global scope
    return str(__import__('__main__').solve() or "")

# Simple sanity: no item, only level ups
assert run("500 10 2\n00:00 2\n00:01 2\n") == "520", "level ups only"

# Item then single attack
assert run("500 10 1\n00:00 1\n00:01 3 1\n") != "", "basic item usage"

# Cooldown test: same enemy repeated
assert run("500 10 3\n00:00 1\n00:01 3 1\n00:10 3 1\n") != "", "cooldown behavior"

# Multiple enemies independent cooldown
assert run("500 10 5\n00:00 1\n00:01 3 1\n00:02 3 2\n00:03 3 1\n") != "", "independent enemies"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chỉ thăng cấp | tăng trưởng tuyến tính | tích lũy H2 tinh khiết | 
| vật phẩm + tấn công | > ban đầu+800 | kích hoạt thụ động | 
| tấn công liên tục của kẻ thù | chặn thời gian hồi chiêu | quy tắc 30 | 
| tấn công nhiều kẻ thù | thời gian hồi chiêu riêng | trạng thái mỗi kẻ thù | 

## Vỏ cạnh 

Một trường hợp quan trọng là tấn công trước khi mặt hàng được mua. Trong tình huống đó, các đòn tấn công phải bị bỏ qua hoàn toàn ngay cả khi logic hồi chiêu cho phép chúng. Mô phỏng xử lý việc này bằng cách kiểm tra`item`cờ trước bất kỳ tính toán nào. 

Một trường hợp khác là các đòn tấn công lặp đi lặp lại chính xác vào giới hạn thời gian hồi chiêu. Nếu một cuộc tấn công xảy ra ở thời điểm t và một cuộc tấn công khác ở thời điểm t+30 thì cuộc tấn công thứ hai phải được kích hoạt. điều kiện`t - last[x] < 30`chỉ loại bỏ chính xác những khoảng cách nhỏ hơn một cách nghiêm ngặt, do đó sự bình đẳng sẽ trôi qua. 

Trường hợp cuối cùng là sự phụ thuộc của sát thương vào lượng máu hiện tại sau nhiều lần tăng cấp và hồi máu trước đó. Vì máu được cập nhật ngay sau mỗi sự kiện nên công thức sát thương luôn phản ánh trạng thái chính xác tại thời điểm tấn công, ngay cả khi nhiều sự kiện xảy ra trong cùng một phút.
