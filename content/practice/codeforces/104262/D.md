---
title: "CF 104262D - Bầu trời thiên thể"
description: "Chúng tôi đang làm việc trên một lưới 2D trong đó cả các ngôi sao và lỗ đen được đặt ở tọa độ nguyên trong một không gian giới hạn nhỏ. Các ngôi sao đại diện cho những điểm chúng ta muốn đếm, trong khi các lỗ đen làm vô hiệu các ngôi sao ở gần."
date: "2026-07-01T21:35:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104262
codeforces_index: "D"
codeforces_contest_name: "UTPC Contest 03-24-23 Div. 1 (Advanced)"
rating: 0
weight: 104262
solve_time_s: 75
verified: true
draft: false
---

[CF 104262D - Bầu trời thiên thể](https://codeforces.com/problemset/problem/104262/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 15s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang làm việc trên một lưới 2D trong đó cả các ngôi sao và lỗ đen được đặt ở tọa độ nguyên trong một không gian giới hạn nhỏ. Các ngôi sao đại diện cho những điểm chúng ta muốn đếm, trong khi các lỗ đen làm vô hiệu các ngôi sao ở gần. 

Mỗi lỗ đen loại bỏ tất cả các ngôi sao bên trong hình vuông 3 × 3 có tâm ở vị trí của nó, nghĩa là mọi ô trong một đơn vị theo cả hai hướng x và y tính từ lỗ đen sẽ trở nên không an toàn. Sau quá trình loại bỏ này, chúng tôi được yêu cầu trả lời nhiều truy vấn hình chữ nhật, mỗi truy vấn hỏi có bao nhiêu ngôi sao còn lại nằm bên trong hình chữ nhật đó. 

Khó khăn chính là các ngôi sao có thể chồng lên nhau, hiệu ứng lỗ đen có thể chồng lên nhau và chúng ta phải trả lời hiệu quả tới 100000 truy vấn sau khi áp dụng tất cả các loại bỏ. 

Phạm vi tọa độ nhỏ, từ 0 đến 999 ở cả hai chiều. Điều này ngay lập tức gợi ý rằng mặc dù số lượng đối tượng lớn nhưng hình học dày đặc nhưng bị giới hạn, thường hướng tới tiền xử lý lưới hoặc tổng tiền tố hơn là cấu trúc động. 

Một cách tiếp cận đơn giản sẽ kiểm tra từng ngôi sao cho mọi truy vấn và cũng kiểm tra xem nó có bị phá hủy bởi lỗ đen nào không. Điều đó sẽ quá chậm vì với 100000 sao và 100000 truy vấn, chúng tôi có thể đạt tới 10^10 thao tác. 

Một trường hợp có cạnh tinh tế là các lỗ đen chồng lên nhau. Ví dụ: nếu một lỗ đen ở (5,5) và một lỗ đen khác ở (6,6), vùng hủy diệt 3×3 của chúng chồng lên nhau rất nhiều. Việc triển khai ngây thơ có thể trừ sai nhiều lần hoặc không đánh dấu được tất cả các ngôi sao bị ảnh hưởng nếu nó chỉ kiểm tra độ gần trực tiếp mà không tổng hợp vùng phủ sóng trước. 

Một trường hợp khác là các ngôi sao chồng lên nhau. Nếu nhiều ngôi sao có cùng tọa độ thì tất cả chúng đều được tính riêng lẻ trừ khi bị phá hủy. Bất kỳ giải pháp dựa trên lưới nào cũng phải tích lũy số lượng thay vì coi tọa độ là tập hợp. 

Cuối cùng, việc phá hủy lỗ đen phải được áp dụng trước khi trả lời các truy vấn. Nếu một người xử lý nhầm các truy vấn trước hoặc đánh giá một cách lười biếng mức độ phá hủy trên mỗi truy vấn, thì kết quả sẽ không nhất quán. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đối với mỗi ngôi sao, chúng tôi quét qua tất cả các lỗ đen để xác định xem liệu nó có nằm bên trong ít nhất một vùng hủy diệt 3×3 hay không. Nếu nó không bị phá hủy, chúng tôi giữ nó. Sau đó, đối với mỗi truy vấn, chúng tôi quét tất cả các ngôi sao còn lại và đếm những ngôi sao đó bên trong hình chữ nhật truy vấn. 

Điều này đúng vì nó trực tiếp tuân theo định nghĩa về sự hủy diệt và đếm. Tuy nhiên, sự phức tạp trở nên nghiêm trọng. Chỉ riêng việc kiểm tra sự phá hủy đã tốn O(NM), với mức 10^5 mỗi cái là 10^10 thao tác. Ngay cả khi được tối ưu hóa một chút, điều này vẫn vượt xa giới hạn. Việc thêm đánh giá truy vấn sẽ làm tăng thêm chi phí. 

Quan sát chính xuất phát từ ràng buộc tọa độ. Vì tất cả tọa độ đều nằm trong lưới 1000×1000 nên chúng ta có thể tính toán trước mọi thứ trên một mảng có kích thước cố định. Thay vì theo dõi các ngôi sao riêng lẻ, chúng tôi tổng hợp chúng thành một lưới tần số. Thay vì mô phỏng sự hủy diệt trên mỗi ngôi sao, chúng tôi đánh dấu một lưới thứ hai gồm các ô “bị chặn” và truyền bá ảnh hưởng của lỗ đen cục bộ. Sau khi cả hai được mã hóa trên lưới, chúng ta có thể tính toán lưới sao hợp lệ cuối cùng và xây dựng tổng tiền tố 2D để trả lời các truy vấn trong O(1). 

Điều này chuyển đổi vấn đề từ xử lý sự kiện sang tiền xử lý lưới tĩnh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(NM + QN) | O(N) | Quá chậm | 
| Lưới + Tổng tiền tố | O(N + M + 10002 + Q) | O(1000²) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi nén tất cả thông tin vào một lưới 1000×1000 cố định để các hoạt động trở thành thời gian không đổi trên mỗi ô. 

1. Xây dựng mảng 2D`stars[x][y]`đếm xem có bao nhiêu ngôi sao tồn tại ở mỗi tọa độ. Điều này là cần thiết vì nhiều ngôi sao có thể chiếm giữ cùng một vị trí và chúng ta phải bảo toàn tính đa dạng. 
2. Xây dựng mảng 2D thứ hai`bad[x][y]`được khởi tạo về 0. Điều này sẽ đánh dấu tất cả các ô bị ảnh hưởng bởi ít nhất một lỗ đen. 
3. Đối với mỗi lỗ đen tại (x, y), đánh dấu tất cả các ô trong hình vuông từ (x−1, y−1) đến (x+1, y+1) là xấu, chú ý kẹp các ranh giới vào giới hạn lưới. Điều này mở rộng hiệu ứng lỗ đen một cách rõ ràng để chúng tôi tránh việc kiểm tra từng sao sau này. Lý do chúng tôi mở rộng ngay lập tức là để chuyển đổi một điều kiện hình học thành một lưới boolean được tính toán trước. 
4. Xây dựng lưới cuối cùng`valid[x][y]`sao cho nó bằng`stars[x][y]`nếu ô không tệ, nếu không thì bằng không. Bước này thu gọn sự phá hủy thành một bộ lọc đơn giản. 
5. Xây dựng tổng tiền tố 2D`valid`. Mỗi ô tiền tố lưu trữ tổng tích lũy của các ngôi sao hợp lệ trong hình chữ nhật từ (0,0) đến (x,y). Điều này cho phép truy vấn hình chữ nhật theo thời gian không đổi. 
6. Đối với mỗi hình chữ nhật truy vấn (x1, y1, x2, y2), chuẩn hóa tọa độ sao cho x1 ≤ x2 và y1 ≤ y2, sau đó tính tổng bằng cách sử dụng loại trừ bao gồm trên lưới tiền tố. 

### Tại sao nó hoạt động 

Sự đúng đắn đến từ việc tách biệt các mối quan tâm. Hiệu ứng lỗ đen chỉ phụ thuộc vào các vùng lân cận cục bộ nên chúng có thể được giải quyết hoàn toàn trước khi xem xét bất kỳ truy vấn nào. Khi chúng tôi chuyển đổi sự phá hủy thành mặt nạ tĩnh trên lưới, vấn đề còn lại hoàn toàn là truy vấn tổng phạm vi 2D trên một ma trận cố định. Tính bất biến của tổng tiền tố đảm bảo rằng mỗi đóng góp của ô được tính chính xác một lần trong bất kỳ hình chữ nhật được truy vấn nào và không có ô nào bị phá hủy đóng góp vì nó được ghi rõ ràng bằng 0 trước khi xây dựng tiền tố. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MAX = 1000

def main():
    N, M, Q = map(int, input().split())

    stars = [[0] * MAX for _ in range(MAX)]
    bad = [[0] * MAX for _ in range(MAX)]

    for _ in range(N):
        x, y = map(int, input().split())
        stars[x][y] += 1

    for _ in range(M):
        x, y = map(int, input().split())
        for dx in (-1, 0, 1):
            for dy in (-1, 0, 1):
                nx, ny = x + dx, y + dy
                if 0 <= nx < MAX and 0 <= ny < MAX:
                    bad[nx][ny] = 1

    valid = [[0] * MAX for _ in range(MAX)]
    for i in range(MAX):
        for j in range(MAX):
            if not bad[i][j]:
                valid[i][j] = stars[i][j]

    pref = [[0] * (MAX + 1) for _ in range(MAX + 1)]

    for i in range(1, MAX + 1):
        for j in range(1, MAX + 1):
            pref[i][j] = (
                valid[i - 1][j - 1]
                + pref[i - 1][j]
                + pref[i][j - 1]
                - pref[i - 1][j - 1]
            )

    def query(x1, y1, x2, y2):
        x1, x2 = min(x1, x2), max(x1, x2)
        y1, y2 = min(y1, y2), max(y1, y2)
        x1 += 1
        y1 += 1
        x2 += 1
        y2 += 1
        return (
            pref[x2][y2]
            - pref[x1 - 1][y2]
            - pref[x2][y1 - 1]
            + pref[x1 - 1][y1 - 1]
        )

    out = []
    for _ in range(Q):
        x1, y1, x2, y2 = map(int, input().split())
        out.append(str(query(x1, y1, x2, y2)))

    print("\n".join(out))

if __name__ == "__main__":
    main()
```Quá trình triển khai bắt đầu bằng cách nén các ngôi sao vào lưới tần số để các bản sao được xử lý một cách tự nhiên. Bước lỗ đen vẽ rõ ràng một vùng lân cận 3 × 3, giúp tránh việc kiểm tra hình học lặp đi lặp lại sau này. Mảng tổng tiền tố được xây dựng với tính năng lập chỉ mục một để đơn giản hóa việc xử lý ranh giới, đó là lý do tại sao tất cả tọa độ truy vấn được dịch chuyển một. Công thức bao gồm-loại trừ là danh tính tổng phạm vi 2D tiêu chuẩn và là lý do duy nhất khiến các truy vấn trở thành O(1). 

Một lỗi phổ biến ở đây là quên rằng tọa độ là bao hàm và nhỏ. Một nguyên nhân khác là không thể kiểm soát được ảnh hưởng của lỗ đen, điều này có thể dẫn đến lỗi chỉ mục. Một vấn đề tế nhị hơn là quên tích lũy sao thay vì ghi đè chúng. 

## Ví dụ đã hoạt động 

Sử dụng đầu vào mẫu: 

Đầu tiên chúng tôi xây dựng lưới hình sao và đánh dấu tất cả các ô bị ảnh hưởng. 

| Bước | Hành động | Trạng thái chính (khái niệm) | 
| --- | --- | --- | 
| 1 | Chèn sao | nhiều tọa độ dân cư | 
| 2 | Áp dụng lỗ đen | 3×3 vùng xung quanh (2,4) và (6,8) bị đánh dấu xấu | 
| 3 | Lọc sao | chỉ còn lại những ngôi sao bên ngoài vùng bị ảnh hưởng | 
| 4 | Xây dựng tiền tố | số tiền tích lũy được chuẩn bị | 
| 5 | Truy vấn | hình chữ nhật được đánh giá qua tiền tố | 

Đối với truy vấn đầu tiên`2 6 3 8`, chỉ có một ngôi sao còn sống sót nằm bên trong hình chữ nhật nên kết quả là 1. 

Đối với truy vấn thứ hai`4 2 9 5`, bốn ngôi sao hợp lệ vẫn còn trong vùng đó sau khi lọc. 

Đối với truy vấn lưới đầy đủ`2 2 9 9`, tất cả các ngôi sao còn sống sót đều được tính, cho kết quả là 8. 

Những dấu vết này cho thấy việc hủy bỏ được áp dụng trên toàn cầu trước khi truy vấn, không phải cho mỗi truy vấn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1000² + N + M + Q) | xây dựng lưới, đánh dấu, tổng tiền tố và truy vấn O(1) | 
| Không gian | O(1000²) | lưới có kích thước cố định cho các ngôi sao, ô xấu và tổng tiền tố | 

Giới hạn 1000×1000 đảm bảo rằng quá trình tiền xử lý bậc hai đủ nhỏ để chạy thoải mái trong giới hạn, ngay cả trong Python. Tất cả các tính toán nặng đều độc lập với N, M và Q sau khi tiền xử lý. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from collections import deque

    MAX = 1000

    N, M, Q = map(int, sys.stdin.readline().split())

    stars = [[0] * MAX for _ in range(MAX)]
    bad = [[0] * MAX for _ in range(MAX)]

    for _ in range(N):
        x, y = map(int, sys.stdin.readline().split())
        stars[x][y] += 1

    for _ in range(M):
        x, y = map(int, sys.stdin.readline().split())
        for dx in (-1, 0, 1):
            for dy in (-1, 0, 1):
                nx, ny = x + dx, y + dy
                if 0 <= nx < MAX and 0 <= ny < MAX:
                    bad[nx][ny] = 1

    valid = [[0] * MAX for _ in range(MAX)]
    for i in range(MAX):
        for j in range(MAX):
            if not bad[i][j]:
                valid[i][j] = stars[i][j]

    pref = [[0] * (MAX + 1) for _ in range(MAX + 1)]
    for i in range(1, MAX + 1):
        for j in range(1, MAX + 1):
            pref[i][j] = valid[i-1][j-1] + pref[i-1][j] + pref[i][j-1] - pref[i-1][j-1]

    def query(x1,y1,x2,y2):
        x1,x2=min(x1,x2),max(x1,x2)
        y1,y2=min(y1,y2),max(y1,y2)
        x1+=1;y1+=1;x2+=1;y2+=1
        return pref[x2][y2]-pref[x1-1][y2]-pref[x2][y1-1]+pref[x1-1][y1-1]

    out=[]
    for _ in range(Q):
        x1,y1,x2,y2=map(int, sys.stdin.readline().split())
        out.append(str(query(x1,y1,x2,y2)))

    return "\n".join(out)

# provided sample
assert run("""12 2 3
1 4
2 5
2 6
4 8
5 3
5 6
5 9
6 2
6 7
7 3
8 3
8 9
2 4
6 8
2 6 3 8
4 2 9 5
2 2 9 9
""") == "1\n4\n8"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Ngôi sao đơn, không có lỗ đen | 1 | đếm cơ bản | 
| Ngôi sao bên trong vùng lỗ đen | 0 | mặt nạ phá hủy | 
| Nhiều lỗ đen chồng chéo | lọc đúng | xử lý chồng chéo | 
| Truy vấn lưới đầy đủ | tổng số người sống sót | tính chính xác của tiền tố | 

## Vỏ cạnh 

Một trường hợp quan trọng là các lỗ đen chồng lên nhau. Nếu một ô ở (5,5) và một ô khác ở (6,6), thuật toán sẽ đánh dấu tất cả các ô ở cả hai vùng lân cận 3×3. Việc đánh dấu thứ hai không hoàn tác hoặc đếm gấp đôi bất cứ điều gì bởi vì`bad[x][y]`là boolean. Điều này đảm bảo mặt nạ cuối cùng ổn định bất kể thứ tự chồng chéo. 

Một trường hợp khác là các ngôi sao nằm chính xác trên ranh giới vùng 3×3 của lỗ đen. Đối với lỗ đen ở (2,2), một ngôi sao ở (1,3) vẫn bị loại bỏ vì nó nằm trong một đơn vị ở cả hai trục. Vòng lặp rõ ràng trên dx và dy đảm bảo bao gồm tất cả các ô biên. 

Cuối cùng, các truy vấn bao trùm toàn bộ lưới sẽ kiểm tra xem tổng tiền tố có tích lũy chính xác tất cả các sao hợp lệ hay không. Cấu trúc bao gồm-loại trừ đảm bảo không tính toán quá mức ngay cả khi nhiều ngôi sao chia sẻ tọa độ.
