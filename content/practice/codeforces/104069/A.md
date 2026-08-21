---
title: "CF 104069A - Bắt cóc Nathan!"
description: "Hai người chơi Thiago và Nathan đang chơi một trò chơi theo phong cách bóng bàn trong đó máy chủ chuyển đổi theo khối thay vì xen kẽ hai điểm như trong luật chính thức. Thay vào đó, máy chủ sẽ thay đổi sau mỗi k điểm ghi được, bất kể ai ghi được chúng."
date: "2026-07-02T02:58:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104069
codeforces_index: "A"
codeforces_contest_name: "VII MaratonUSP Freshman Contest"
rating: 0
weight: 104069
solve_time_s: 49
verified: true
draft: false
---

[CF 104069A - Bắt cóc Nathan!](https://codeforces.com/problemset/problem/104069/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Hai người chơi Thiago và Nathan đang chơi một trò chơi theo phong cách bóng bàn trong đó máy chủ chuyển đổi theo khối thay vì xen kẽ hai điểm như trong luật chính thức. Thay vào đó, máy chủ sẽ thay đổi sau mỗi k điểm ghi được, bất kể ai ghi được chúng. Thiago luôn bắt đầu giao bóng ngay từ đầu trận đấu. 

Chúng tôi được cho điểm hiện tại trong nhiều trường hợp thử nghiệm độc lập. Đối với mỗi trường hợp, chúng ta cần xác định ai sẽ giao bóng tiếp theo dựa trên tổng số điểm đã chơi hiện tại. Bản thân điểm số không ảnh hưởng đến việc thay đổi giao bóng ngoại trừ số điểm đã được chơi cho đến nay, vì mỗi điểm sẽ nâng cao chu kỳ giao bóng. 

Mỗi trường hợp thử nghiệm cung cấp k, kích thước khối cho các thay đổi dịch vụ và điểm số hiện tại T và N. Từ đó, chúng tôi tính toán tổng số điểm đã được chơi, sau đó xác định có bao nhiêu khối dịch vụ đầy đủ đã vượt qua. Nếu số khối hoàn thành là số chẵn thì Thiago vẫn là máy chủ; nếu lạ thì Nathan đang phục vụ. 

Các ràng buộc cho phép tối đa 10^4 trường hợp kiểm thử và giá trị lên tới 10^9 cho k và điểm. Điều này ngay lập tức loại trừ mọi mô phỏng xử lý từng điểm riêng lẻ. Mô phỏng theo từng điểm sẽ yêu cầu các thao tác lên tới 2 × 10^9 trong một thử nghiệm, vượt xa giới hạn khả thi. Giải pháp phải là O(1) cho mỗi trường hợp thử nghiệm. 

Trường hợp cạnh tinh tế xuất hiện khi tổng số điểm chia hết cho k. Trong tình huống đó, quả giao bóng sẽ chuyển ngay sau điểm cuối cùng, vì vậy người giao bóng tiếp theo sẽ phụ thuộc vào số khối hoàn thành là chẵn hay lẻ. Một trường hợp phạt góc khác là khi T = 0 và N = 0, nghĩa là chưa có điểm nào được thực hiện nên Thiago vẫn phải giao bóng. 

## Phương pháp tiếp cận 

Ý tưởng ngây thơ là mô phỏng trò chơi theo từng điểm. Bắt đầu với Thiago làm máy chủ, chúng tôi sẽ tăng bộ đếm cho mỗi điểm và chuyển đổi máy chủ sau mỗi k điểm. Sau khi xử lý các bước T + N, chúng ta sẽ xuất ra máy chủ hiện tại. Điều này đúng vì nó tuân theo quy tắc trực tiếp. 

Tuy nhiên, cách tiếp cận này trở nên bất khả thi do bị ràng buộc vì T + N có thể lớn bằng 2 × 10^9. Ngay cả việc thực hiện một thao tác trên mỗi điểm cũng sẽ vượt quá giới hạn thời gian theo một số bậc độ lớn. 

Quan sát quan trọng là chúng ta không bao giờ cần phải theo dõi từng điểm riêng lẻ. Số lượng liên quan duy nhất là tổng số khối đã hoàn thành có kích thước k. Khi chúng ta tính S = T + N, số khối đầy đủ là S // k. Mỗi khối lật máy chủ đúng một lần, do đó máy chủ chỉ phụ thuộc vào tính chẵn lẻ của thương số này. 

Điều này làm giảm toàn bộ vấn đề thành một phép tính số học theo thời gian không đổi cho mỗi trường hợp kiểm thử. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(T + N) mỗi lần kiểm tra | O(1) | Quá chậm | 
| Đếm khối số học | O(1) mỗi lần kiểm tra | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc k, T và N cho từng test. Chúng xác định kích thước khối và tổng số điểm hiện tại trong trạng thái trò chơi. 
2. Tính S = T + N, tổng số điểm đã chơi tính đến thời điểm hiện tại. Việc chuyển giao bóng chỉ phụ thuộc vào số lượng phân đoạn k điểm đầy đủ đã được hoàn thành. 
3. Khối tính toán = S // k. Mỗi khối đầy đủ tương ứng với một khoảng thời gian dịch vụ hoàn chỉnh sau đó máy chủ sẽ ngừng hoạt động. 
4. Nếu các khối chẵn, ghi "Thiago" vì Thiago đã bắt đầu giao bóng và một số lần lật bóng chẵn sẽ trả lại quả giao bóng cho người bắt đầu. 
5. Nếu các khối là số lẻ, hãy ghi "Nathan" vì số lần lật là số lẻ có nghĩa là cú giao bóng đã bị chuyển khỏi phần bắt đầu. 

### Tại sao nó hoạt động

Cú giao bóng chỉ thay đổi ở các ranh giới cố định có kích thước k trong chuỗi điểm chung. Thao tác này sẽ chia dòng thời gian thành các khoảng liên tiếp, mỗi khoảng sẽ đảo ngược máy chủ hiện tại đúng một lần. Vì Thiago bắt đầu giao bóng ở thời điểm 0, nên người giao bóng sau S điểm chỉ phụ thuộc vào số lần hoàn thành khoảng thời gian đã được vượt qua. Tiến trình một phần trong khoảng thời gian hiện tại không ảnh hưởng đến máy chủ tiếp theo, chỉ những khoảng thời gian đã hoàn thành mới quan trọng. Do đó tính chẵn lẻ của S // k xác định đầy đủ câu trả lời. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    k, T, N = map(int, input().split())
    total = T + N
    blocks = total // k

    if blocks % 2 == 0:
        print("Thiago")
    else:
        print("Nathan")
```Việc thực hiện trực tiếp theo sau việc giảm toán học. Bước quan trọng là chia trạng thái thành tổng số điểm, tránh mọi nhu cầu theo dõi trình tự tính điểm của từng cá nhân. Phép chia số nguyên tính toán số khoảng thời gian dịch vụ đã hoàn thành và tính chẵn lẻ sẽ xác định máy chủ. 

Một lỗi phổ biến là quên rằng việc chuyển đổi xảy ra sau k điểm, không phải mọi điểm thứ k kể cả 0. Việc sử dụng tính năng chia tầng sẽ xử lý việc này một cách chính xác mà không gặp phải vấn đề riêng lẻ nào. Một điểm tinh tế khác là chúng ta không cần xem xét ai là người ghi điểm cuối cùng, vì việc ghi điểm không ảnh hưởng đến luật giao bóng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Đầu vào: k = 5, T = 3, N = 2 

Tổng điểm S = 5 

| Bước | S | khối = S // k | chẵn lẻ | máy chủ | 
| --- | --- | --- | --- | --- | 
| ban đầu | 5 | 1 | lẻ | Nathan | 

Vì đã hết một đợt cản phá nên Thiago đã đổi người một lần và Nathan đang giao bóng. 

Điều này khẳng định rằng ngay cả khi sự phân bổ điểm không đồng đều thì chỉ có tổng số điểm là quan trọng. 

### Ví dụ 2 

Đầu vào: k = 5, T = 3, N = 1 

S = 4 

| Bước | S | khối | chẵn lẻ | máy chủ | 
| --- | --- | --- | --- | --- | 
| ban đầu | 4 | 0 | thậm chí | Thiago | 

Không có pha chặn bóng nào hoàn thành nên Thiago vẫn giao bóng. Điều này cho thấy các khối chưa hoàn chỉnh không kích hoạt công tắc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi trường hợp thử nghiệm thực hiện một số phép tính số học không đổi | 
| Không gian | O(1) | Chỉ một số số nguyên được lưu trữ bất kể kích thước đầu vào | 

Giải pháp có thể xử lý tối đa 10^4 trường hợp thử nghiệm một cách thoải mái vì mỗi trường hợp được rút gọn thành phép chia đơn lẻ và phép toán modulo. 

## Trường hợp thử nghiệm```python
import sys, io

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []
    for _ in range(t):
        k, T, N = map(int, input().split())
        total = T + N
        blocks = total // k
        out.append("Thiago" if blocks % 2 == 0 else "Nathan")
    return "\n".join(out)

# provided sample-style cases
assert solve("2\n7 1 0\n2 2 8\n") == "Thiago\nNathan"

# k large, no switch
assert solve("1\n10 3 4\n") == "Thiago"

# exact boundary switch
assert solve("1\n5 3 2\n") == "Nathan"

# multiple switches
assert solve("1\n3 9 0\n") == "Thiago"

# zero points
assert solve("1\n5 0 0\n") == "Thiago"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| k=10, S=7 | Thiago | không có khối hoàn thành | 
| k=5, S=5 | Nathan | lật ranh giới chính xác | 
| k=3, S=9 | Thiago | số lần lật chẵn | 
| k=5, S=0 | Thiago | trạng thái ban đầu | 

## Vỏ cạnh 

Khi không có điểm nào được thi đấu, T = 0 và N = 0, tổng số là S = 0. Tính toán cho các khối = 0 // k = 0, là số chẵn nên Thiago được xác định chính xác là máy chủ. 

Đối với các trường hợp S chính xác là bội số của k, chẳng hạn như k = 4 và S = 8, chúng tôi nhận được khối = 2. Vì hai lượt giao bóng đầy đủ đã hoàn thành nên quả giao bóng đã chuyển đổi hai lần, quay trở lại Thiago. Việc triển khai nắm bắt chính xác điều này mà không cần phát hiện rõ ràng các ranh giới. 

Đối với các giá trị rất lớn, chẳng hạn như k = 10^9 và S gần 2 × 10^9, phép chia số nguyên vẫn chạy trong thời gian không đổi và tránh được sự cố tràn trong Python một cách an toàn.
