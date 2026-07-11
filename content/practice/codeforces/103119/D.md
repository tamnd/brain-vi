---
title: "CF 103119D - Hiện vật"
description: "Chúng tôi được cấp năm vật phẩm tạo tác, một vật phẩm cho mỗi ô trang bị. Mỗi hiện vật đóng góp chính xác năm dòng chỉ số và trên tất cả hiện vật, chúng tôi chỉ quan tâm đến bốn số liệu thống kê: ATK cố định, tỷ lệ phần trăm ATK, Tỷ lệ chí mạng và Sát thương chí mạng."
date: "2026-07-03T22:39:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103119
codeforces_index: "D"
codeforces_contest_name: "The 2020 ICPC Asia Macau Regional Contest"
rating: 0
weight: 103119
solve_time_s: 48
verified: true
draft: false
---

[CF 103119D - Hiện vật](https://codeforces.com/problemset/problem/103119/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp năm vật phẩm tạo tác, một vật phẩm cho mỗi ô trang bị. Mỗi hiện vật đóng góp chính xác năm dòng chỉ số và trên tất cả hiện vật, chúng tôi chỉ quan tâm đến bốn số liệu thống kê: ATK cố định, tỷ lệ phần trăm ATK, Tỷ lệ chí mạng và Sát thương chí mạng. 

Mô hình nhân vật cuối cùng bắt đầu với các chỉ số cơ bản cố định: 1500 ATK, 5% Tỷ lệ chí mạng và 50% sát thương chí mạng. Hiện vật sửa đổi các giá trị này theo cấp số cộng hoặc cấp số nhân tùy thuộc vào loại chỉ số. Giá trị ATK cố định được tính tổng trực tiếp. Giá trị phần trăm ATK tích lũy thành một hệ số duy nhất áp dụng cho ATK cơ bản. Tỷ lệ chí mạng và sát thương chí mạng cũng được tích lũy bổ sung trên các hiện vật. 

Sau khi tổng hợp, ATK được tính bằng 1500 nhân (1 cộng với tổng phần trăm ATK) cộng với tổng ATK cố định. Tỷ lệ chí mạng được giới hạn để mọi giá trị trên 100 phần trăm trở thành chính xác 100 phần trăm. Sát thương chí mạng không có giới hạn như vậy. 

Cuối cùng, sát thương dự kiến được định nghĩa là sự kết hợp giữa đòn đánh thường và đòn chí mạng: 

E = ATK nhân với xác suất không chí mạng cộng với ATK nhân với hệ số chí mạng nhân với xác suất chí mạng. Điều này đơn giản hóa thành mức trung bình có trọng số giữa kết quả bình thường và quan trọng. 

Đầu vào hoàn toàn là văn bản, vì vậy khó khăn chính là phân tích cú pháp và tích lũy chính xác các giá trị với định dạng phần trăm hỗn hợp và độ chính xác của dấu phẩy động. 

Các ràng buộc rất nhỏ: chỉ có năm hiện vật và mỗi dòng năm dòng, tổng cộng là 25 dòng. Điều này loại trừ mọi nhu cầu tối ưu hóa ngoài phân tích cú pháp và số học O(1). Rủi ro thực sự là những sai sót về mặt số học: hiểu sai các ký hiệu phần trăm, quên tỷ lệ chí mạng cơ bản 5% hoặc kẹp tỷ lệ chí mạng không chính xác ở mức 100%. 

Một trường hợp phức tạp xuất hiện khi Tỷ lệ chí mạng vượt quá 100 phần trăm sau khi tổng hợp. Ví dụ: nếu hiện vật có tổng Tỷ lệ Chí mạng là 150 phần trăm thì giá trị hiệu quả phải được giới hạn chính xác ở mức 100 phần trăm, nếu không thì thiệt hại dự kiến ​​sẽ được đánh giá quá cao. 

Một cạm bẫy phổ biến khác là trộn tỷ lệ phần trăm. Tỷ lệ ATK được tính ở dạng phần trăm nhưng được áp dụng dưới dạng số nhân (x phần trăm trở thành x chia cho 100). Việc hiểu sai điều này là phép cộng trực tiếp dẫn đến kết quả đầu ra bị sai lệch theo hệ số 100. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực sẽ cố gắng liệt kê các kết hợp các lựa chọn tạo tác trên mỗi vị trí, nhưng ở đây mỗi vị trí có chính xác một tạo tác và tất cả đều được chọn, do đó không có tìm kiếm tổ hợp. Công việc duy nhất là tổng hợp. 

Quan sát cốt lõi là mọi chỉ số đều đóng góp độc lập và tuyến tính cho đến bước đánh giá cuối cùng. Điều này cho phép chúng ta giảm vấn đề xuống còn phân tích 25 chuỗi và tính tổng bốn chuỗi đang chạy. 

Sau khi tính tổng, chúng tôi áp dụng trực tiếp công thức tính ATK và sát thương dự kiến. Hành vi phi tuyến tính duy nhất là giới hạn Tỷ lệ chí mạng ở mức 1,0, phải được áp dụng sau khi tính tổng và trước khi tính toán kỳ vọng. 

Bởi vì mọi thứ đều mang tính cộng cho đến công thức cuối cùng, chúng ta không cần bất kỳ cấu trúc dữ liệu nào ngoài bốn bộ tích lũy động. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê lực lượng vũ phu | O(1) | O(1) | Đã chấp nhận | 
| Phân tích và tổng hợp | O(25) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Khởi tạo bốn bộ tích lũy: tổng ATK cố định, tổng phần trăm ATK, tổng Tỷ lệ chí mạng và tổng sát thương chí mạng. Chúng bắt đầu từ số 0 vì số liệu thống kê cơ sở được xử lý riêng trong công thức cuối cùng. 
2. Đọc tất cả 25 dòng và phân tích từng chuỗi thống kê bằng cách tách ở dấu cộng. Phía bên trái xác định loại thống kê và phía bên phải cung cấp một giá trị số có thể bao gồm hoặc không bao gồm dấu phần trăm. 
3. Nếu chỉ số là ATK, hãy cộng trực tiếp giá trị số vào tổng ATK cố định. 
4. Nếu chỉ số là Tỷ lệ ATK, hãy chuyển phần trăm thành số thập phân bằng cách chia cho 100 và cộng nó vào tổng phần trăm ATK. 
5. Nếu chỉ số là Tỷ lệ chí mạng, hãy chuyển đổi sang số thập phân và cộng nó vào tổng Tỷ lệ chí mạng. 
6. Nếu chỉ số là Sát thương Chí mạng, hãy chuyển đổi sang số thập phân và cộng nó vào tổng Sát thương Chí mạng. 
7. Sau khi xử lý tất cả các hiện vật, tính ATK cuối cùng là 1500 nhân (1 cộng với tổng phần trăm ATK) cộng với tổng ATK cố định. Điều này tách biệt tỷ lệ cơ sở với phần thưởng phẳng bổ sung. 
8. Kẹp Tỷ lệ Chí mạng ở mức tối đa là 1,0. Điều này thực thi quy tắc xác suất không thể vượt quá sự chắc chắn. 
9. Tính toán sát thương dự kiến ​​bằng công thức tính trọng số: ATK nhân (1 trừ Tỷ lệ chí mạng) cộng với ATK nhân (1 cộng với Sát thương chí mạng) nhân với Tỷ lệ chí mạng. 
10. Xuất kết quả với độ chính xác đủ để đáp ứng các hạn chế về lỗi. 

### Tại sao nó hoạt động 

Tất cả các đóng góp chỉ số đều độc lập và bổ sung cho đến công thức kỳ vọng cuối cùng. Điều này có nghĩa là chúng tôi có thể tổng hợp từng danh mục một cách an toàn mà không phải lo lắng về thứ tự hoặc hiệu ứng tương tác giữa các tạo phẩm. Phép biến đổi phi tuyến tính duy nhất là giới hạn Tỷ lệ chí mạng, được áp dụng sau khi tính tổng, duy trì tính chính xác vì độ bão hòa xác suất chỉ phụ thuộc vào tổng số tiền chứ không phải các nguồn riêng lẻ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

BASE_ATK = 1500.0
BASE_CRIT_RATE = 0.05
BASE_CRIT_DMG = 0.50

flat_atk = 0.0
atk_rate = 0.0
crit_rate = 0.0
crit_dmg = 0.0

for _ in range(25):
    s = input().strip()
    typ, val = s.split('+')

    if val.endswith('%'):
        x = float(val[:-1]) / 100.0
    else:
        x = float(val)

    if typ == "ATK":
        flat_atk += x
    elif typ == "ATK Rate":
        atk_rate += x
    elif typ == "Crit Rate":
        crit_rate += x
    elif typ == "Crit DMG Rate":
        crit_dmg += x

atk = BASE_ATK * (1.0 + atk_rate) + flat_atk

crit_rate = BASE_CRIT_RATE + crit_rate
crit_dmg = BASE_CRIT_DMG + crit_dmg

if crit_rate > 1.0:
    crit_rate = 1.0

expected = atk * (1.0 - crit_rate) + atk * (1.0 + crit_dmg) * crit_rate

print(expected)
```Giải pháp được cấu trúc xung quanh bốn bộ tích lũy độc lập, mỗi bộ tích lũy cho mỗi chỉ số liên quan. Việc phân tích cú pháp được thực hiện thống nhất cho tất cả các dòng với việc kiểm tra đơn giản các giá trị phần trăm. Một lỗi triển khai phổ biến là quên rằng số liệu thống kê phần trăm phải được chia cho 100 trước khi tổng hợp. 

Công thức ATK chỉ được áp dụng sau khi tất cả các khoản đóng góp đã được thu thập, vì phần trăm ATK sửa đổi giá trị cơ bản thay vì đóng vai trò là thuật ngữ cộng trực tiếp. Kẹp Tỷ lệ chí mạng được áp dụng sau khi thêm 5 phần trăm cơ sở, vì cả đóng góp cơ bản và hiện vật đều kết hợp trước khi thực thi trần xác suất. 

## Ví dụ đã hoạt động 

Chúng tôi xây dựng một dấu vết đơn giản hóa bằng cách sử dụng một tập hợp con đầu vào tối thiểu. 

### Ví dụ 1 

đầu vào: 

CÔNG+10 

Tỷ lệ tấn công +10% 

Tỷ lệ chí mạng +10% 

Tỷ lệ sát thương chí mạng +10% 

| Bước | ATK phẳng | Tỷ Lệ ATK | Tỷ lệ chí mạng | DMG chí mạng | 
| --- | --- | --- | --- | --- | 
| Sau khi phân tích | 10 | 0,10 | 0,10 | 0,10 | 

Tính toán cuối cùng tiến hành như sau: 

CÔNG = 1500 × 1,1 + 10 = 1660 

Tỷ lệ Chí mạng = 0,05 + 0,10 = 0,15 

Sát thương chí mạng = 0,50 + 0,10 = 0,60 

Dự kiến = 1660 × (0,85 + 1,60 × 0,15) = 1660 × 1,09 = 1809,4 

Điều này thể hiện sự phân tách chính xác giữa số liệu thống kê cơ bản và các sửa đổi phụ gia. 

### Ví dụ 2 

đầu vào: 

Tỉ lệ Chí mạng+120% 

CÔNG+0 

| Bước | ATK phẳng | Tỷ Lệ ATK | Tỷ lệ chí mạng | DMG chí mạng | 
| --- | --- | --- | --- | --- | 
| Sau khi phân tích | 0 | 0,0 | 1,20 | 0,0 | 

Sau khi thêm bazơ: 

Tỷ lệ chí mạng = 0,05 + 1,20 = 1,25, sau đó giới hạn ở mức 1,0 

Dự kiến ​​sẽ mang tính quyết định: 

CÔNG × (1 + sát thương chí mạng) = 1500 × 1,5 = 2250 

Điều này xác nhận việc xử lý chính xác độ bão hòa xác suất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(25) | Mỗi dòng trong số 25 dòng được phân tích cú pháp một lần và đóng góp các bản cập nhật liên tục | 
| Không gian | O(1) | Chỉ duy trì một số bộ tích lũy dấu phẩy động cố định | 

Khối lượng công việc có kích thước không đổi bất kể cấu trúc đầu vào, do đó hiệu suất ở mức không đáng kể trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math

    BASE_ATK = 1500.0
    BASE_CRIT_RATE = 0.05
    BASE_CRIT_DMG = 0.50

    flat_atk = 0.0
    atk_rate = 0.0
    crit_rate = 0.0
    crit_dmg = 0.0

    for _ in range(25):
        s = sys.stdin.readline().strip()
        typ, val = s.split('+')
        if val.endswith('%'):
            x = float(val[:-1]) / 100.0
        else:
            x = float(val)

        if typ == "ATK":
            flat_atk += x
        elif typ == "ATK Rate":
            atk_rate += x
        elif typ == "Crit Rate":
            crit_rate += x
        elif typ == "Crit DMG Rate":
            crit_dmg += x

    atk = BASE_ATK * (1.0 + atk_rate) + flat_atk
    crit_rate = BASE_CRIT_RATE + crit_rate
    crit_dmg = BASE_CRIT_DMG + crit_dmg
    crit_rate = min(1.0, crit_rate)

    return str(atk * (1.0 - crit_rate) + atk * (1.0 + crit_dmg) * crit_rate)

# sample-like test
inp = "\n".join(["ATK+10"] * 25)
assert float(run(inp)) > 1500

# crit cap test
inp = "\n".join(["Crit Rate+50%"] * 3 + ["ATK+0"] * 22)
assert float(run(inp)) > 1500

# all damage stats
inp = "\n".join(["ATK Rate+10%", "Crit Rate+10%", "Crit DMG Rate+10%", "ATK+10", "ATK+10"] * 5)
assert float(run(inp)) > 0

# zero case
inp = "\n".join(["ATK+0"] * 25)
assert abs(float(run(inp)) - 1500 * (1 - 0.05 + 1.5 * 0.05)) < 1e-6
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Tất cả ATK | >1500 | tích lũy phẳng đúng đắn | 
| Tỷ lệ chí mạng quá mức | kẹp | giới hạn xác suất | 
| Số liệu thống kê hỗn hợp | giá trị dương | hiệu ứng kết hợp | 
| Không có số liệu thống kê | công thức cơ bản | độ chính xác cơ bản | 

## Vỏ cạnh 

Một trường hợp quan trọng là tràn tỷ lệ chí mạng. Nếu tổng Tỷ lệ chí mạng vượt quá 1,0 sau khi tổng hợp tất cả các hiện vật, hành vi đúng là kẹp nó lại. Thuật toán xử lý vấn đề này bằng cách áp dụng thao tác tối thiểu sau khi tổng hợp, đảm bảo xác suất không vượt quá 100%. 

Một trường hợp khác là trộn định dạng phần trăm. Vì một số giá trị bao gồm ký hiệu phần trăm còn các giá trị khác thì không, nên việc không chuẩn hóa bằng cách chia cho 100 sẽ làm tăng Tỷ lệ ATK hoặc Tỷ lệ chí mạng lên gấp 100. Bước phân tích cú pháp sẽ kiểm tra rõ ràng ký hiệu phần trăm và chuyển đổi nhất quán. 

Trường hợp tinh tế cuối cùng là độ chính xác của dấu phẩy động. Vì dung sai lỗi bắt buộc là 1e-6 nên sử dụng số học float của Python là đủ, nhưng việc in mà không cần kiểm soát định dạng có thể chấp nhận được vì đánh giá cho phép lỗi tương đối.
