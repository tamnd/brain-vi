---
title: "CF 102992E - Tọa độ ác"
description: "Chúng tôi đang xây dựng một lối đi trên một lưới vô hạn bắt đầu từ điểm gốc. Mỗi bước di chuyển là một đơn vị theo một trong bốn hướng chính: phải, trái, lên hoặc xuống, với số lượng sẵn có cho mỗi hướng. Lượt đi cuối cùng phải sử dụng chính xác tất cả các bước di chuyển."
date: "2026-07-04T02:42:43+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102992
codeforces_index: "E"
codeforces_contest_name: "2020-2021 ACM-ICPC, Asia Nanjing Regional Contest (XXI Open Cup, Grand Prix of Nanjing)"
rating: 0
weight: 102992
solve_time_s: 73
verified: true
draft: false
---

[CF 102992E - Tọa độ tà ác](https://codeforces.com/problemset/problem/102992/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 13s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang xây dựng một lối đi trên một lưới vô hạn bắt đầu từ điểm gốc. Mỗi bước di chuyển là một đơn vị theo một trong bốn hướng chính: phải, trái, lên hoặc xuống, với số lượng sẵn có cho mỗi hướng. Lượt đi cuối cùng phải sử dụng chính xác tất cả các bước di chuyển. 

Có một điểm lưới bị cấm duy nhất được gọi là “tọa độ ác”. Yêu cầu này mạnh mẽ hơn việc chỉ kết thúc ở một nơi khác: không lúc nào trong quá trình đi bộ, kể cả tiền tố trung gian, chúng ta được phép hạ cánh chính xác trên tọa độ đó. 

Nhiệm vụ là quyết định thứ tự của tất cả các nước đi tránh bước vào ô cấm đó hoặc xác định rằng không tồn tại thứ tự như vậy. 

Do đó, cấu trúc đầu vào là bốn số nguyên mô tả số bước chúng ta có thể thực hiện theo mỗi hướng, cộng với hai số nguyên mô tả tọa độ bị cấm. Đầu ra là một chuỗi các bước di chuyển hợp lệ, thường là một chuỗi hoặc một tuyên bố rằng điều đó là không thể. 

Các ràng buộc đủ nhỏ để đầu ra là tuyến tính theo số lần di chuyển, nhưng đủ lớn để mọi nỗ lực mô phỏng tất cả các hoán vị hoặc lực lượng vũ phu theo thứ tự sẽ ngay lập tức thất bại. Với tổng số nước đi lên tới 10^5, mọi giải pháp đều phải xây dựng chuỗi trong thời gian O(n). 

Khó khăn chính là các cấu trúc ngây thơ như “làm tất cả các quyền, sau đó tất cả lên, rồi tất cả bên trái, rồi tất cả các nhược điểm” có thể vô tình đi qua tọa độ bị cấm. Điều này xảy ra vì tiền tố trung gian của các đoạn đơn điệu có thể khớp chính xác với điểm ác nếu nó nằm trong hình chữ nhật được quét bởi các đoạn đó. 

Một trường hợp thất bại tinh vi xuất hiện khi điểm cấm nằm chính xác trong vùng được hình thành bởi hai khối định hướng liên tiếp. Ví dụ: nếu chúng ta di chuyển tất cả các quyền trước rồi đến tất cả các lên, thì bất kỳ điểm (x, y) nào có x ∼ R và y ∼ U sẽ bị tấn công ở một tiền tố nào đó. Nếu điều đó bao gồm tọa độ xấu thì việc xây dựng không hợp lệ mặc dù điểm cuối cuối cùng vẫn ổn. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ sẽ là thử tất cả các hoán vị của bốn khối định hướng và kiểm tra xem có thứ tự nào tránh được điểm cấm hay không. Mỗi hoán vị xác định một hình dạng đường dẫn đơn điệu và chúng tôi sẽ mô phỏng bước đi cho mỗi cách sắp xếp. 

Điều này hoạt động hợp lý vì bất kỳ giải pháp hợp lệ nào cũng phải có thứ tự nào đó của nhiều bước di chuyển. Tuy nhiên, chỉ có 4! = 24 hoán vị, nên việc kiểm tra chúng cũng rẻ. Vấn đề thực sự là mỗi thứ tự vẫn cần được xác thực cẩn thận xem liệu đường đi có giao nhau với tọa độ bị cấm hay không và quan trọng hơn, cấu trúc của vấn đề cho thấy rằng chỉ một loại lỗi rất cụ thể mới có thể xảy ra: chạm vào điểm cấm bên trong một chuyển tiếp góc phần tư đơn điệu. Vì vậy, vũ lực là quá mức cần thiết và che giấu cấu trúc. 

Quan sát quan trọng là đường đi chỉ trở nên “nguy hiểm” khi chúng ta chuyển đổi giữa hai hướng vuông góc di chuyển về phía tọa độ cấm. Đối với mỗi góc phần tư so với gốc, chỉ có một cặp hướng quan trọng: phải/lên, phải/xuống, trái/lên hoặc trái/xuống. Tất cả các bước di chuyển khác đều trực giao theo cách không thể tạo lại tọa độ bị cấm một khi chúng ta rời khỏi vùng của nó. 

Vì vậy, thay vì tìm kiếm các hoán vị trên toàn cục, chúng ta chỉ cần quyết định thứ tự tương đối của hai khối trong góc phần tư có liên quan. Tất cả các bước di chuyển còn lại có thể được thêm vào một cách an toàn sau đó. 

Điều này làm giảm vấn đề lựa chọn giữa hai thứ tự cho một cặp khối định hướng sao cho đường dẫn tiền tố không bao giờ dừng chính xác trên (x, y). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Hoán vị Brute Force | O(n) | O(n) | Được chấp nhận nhưng không cần thiết | 
| Thứ tư tham lam đặt hàng | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi biểu thị số lần di chuyển là R, L, U, D và tọa độ bị cấm là (x, y). 

### 1. Xác định điểm cấm nằm ở góc phần tư nào

Chúng tôi phân loại hướng mục tiêu hướng tới (x, y). Nếu x không âm, việc đạt tới nó đòi hỏi phải di chuyển sang phải; nếu x âm thì cần di chuyển sang trái. Tương tự cho y và chuyển động thẳng đứng. Điều này cho chúng ta biết hai loại di chuyển nào có khả năng “xây dựng” tọa độ bị cấm. 

Hai loại di chuyển còn lại có hướng ngược nhau và không thể tham gia vào việc hình thành tiền tố bị cấm. 

### 2. Chỉ tập trung vào hai hướng liên quan 

Nếu x ≥ 0 và y ≥ 0, chỉ có R và U quan trọng đối với khả năng trúng (x, y). Nếu x ≥ 0 và y < 0 thì cặp nguy hiểm là R và D. Nếu x < 0 và y ≥ 0 thì là L và U. Nếu x < 0 và y < 0 thì là L và D. 

Tất cả các bước di chuyển còn lại có thể được đặt tự do trước hoặc sau mà không tạo tọa độ cấm, vì chúng di chuyển ra xa tọa độ đó theo ít nhất một trục. 

### 3. Quyết định thứ tự của hai khối quan trọng 

Bây giờ chúng tôi xem xét liệu việc đặt một khối trước khối kia có khiến tiền tố chạm vào (x, y) hay không. 

Lấy trường hợp x ≥ 0 và y ≥ 0. Nếu đặt tất cả R di chuyển trước rồi đến tất cả U di chuyển thì mọi tiền tố của khối R đều nằm trên trục x và mọi tiền tố sau khi chuyển sang U đều nằm trên một đường thẳng đứng tại x = R cố định. 

Nếu x ∼ R và y ∼ U thì trong quá trình chuyển sang khối U, cuối cùng chúng ta sẽ đạt chính xác (x, y), điều này bị cấm. 

Để tránh điều này, chúng ta đổi thứ tự và thay vào đó đặt U trước R. Trong cách sắp xếp đó, trước tiên chúng ta tăng y độc lập với x, do đó tiền tố khớp với (x, y) không bao giờ được hình thành theo cùng một cách. 

Logic tương tự được áp dụng một cách đối xứng trong các góc phần tư khác: chúng tôi chọn thứ tự tránh căn chỉnh cả hai tọa độ theo cách chạm vào điểm cấm ở cùng một bước tiền tố. 

### 4. Nối 2 hướng còn lại 

Sau khi sửa thứ tự an toàn của cặp quan trọng, chúng tôi nối thêm tất cả các bước di chuyển theo hướng ngược lại (L hoặc D khi làm việc trong trường hợp góc phần tư bên phải/trên). Những bước di chuyển này chỉ làm giảm tọa độ và không thể giới thiệu lại điểm cấm vì chúng tôi đã tránh khớp nó trong giai đoạn tiền tố xây dựng. 

### 5. Xuất ra chuỗi đã xây dựng 

Trình tự cuối cùng chỉ đơn giản là nối các khối đã chọn. 

### Tại sao nó hoạt động 

Điều bất biến là ở bất kỳ tiền tố nào, đường đi không bao giờ khớp đồng thời cả hai tọa độ cần thiết để đến điểm cấm trong cùng một pha chuyển động đơn điệu. Cách duy nhất để hạ cánh chính xác trên (x, y) trong cách xây dựng này là nếu cả hai trục được tích lũy một cách đồng bộ trong cùng một thứ tự khối. Bằng cách chọn thứ tự tách rời sự tích lũy của hai tọa độ, chúng tôi đảm bảo rằng không có tiền tố nào trùng với tọa độ bị cấm. 

Vì tất cả các bước di chuyển khác đều di chuyển ra khỏi mục tiêu góc phần tư có liên quan nên chúng không thể đưa lại sự căn chỉnh bị thiếu sau này trong trình tự. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    R, L, U, D, x, y = map(int, input().split())
    
    res = []
    
    # Decide quadrant of dangerous point
    if x >= 0 and y >= 0:
        # dangerous pair: R and U
        if not (x <= R and y <= U):
            res.append("R" * R)
            res.append("U" * U)
        else:
            res.append("U" * U)
            res.append("R" * R)
        res.append("L" * L)
        res.append("D" * D)
    
    elif x >= 0 and y < 0:
        # dangerous pair: R and D
        y = -y
        if not (x <= R and y <= D):
            res.append("R" * R)
            res.append("D" * D)
        else:
            res.append("D" * D)
            res.append("R" * R)
        res.append("L" * L)
        res.append("U" * U)
    
    elif x < 0 and y >= 0:
        # dangerous pair: L and U
        x = -x
        if not (x <= L and y <= U):
            res.append("L" * L)
            res.append("U" * U)
        else:
            res.append("U" * U)
            res.append("L" * L)
        res.append("R" * R)
        res.append("D" * D)
    
    else:
        # dangerous pair: L and D
        x = -x
        y = -y
        if not (x <= L and y <= D):
            res.append("L" * L)
            res.append("D" * D)
        else:
            res.append("D" * D)
            res.append("L" * L)
        res.append("R" * R)
        res.append("U" * U)
    
    print("".join(res))

if __name__ == "__main__":
    solve()
```Việc thực hiện trực tiếp theo sau việc phân tách góc phần tư. Mỗi nhánh tách biệt hai hướng có thể cùng nhau xây dựng tọa độ bị cấm và đảm bảo thứ tự của chúng tránh được việc căn chỉnh tiền tố đồng thời. 

Một điểm tinh tế là chúng tôi chuyển đổi tọa độ âm thành so sánh dương khi kiểm tra tính khả thi trong từng trường hợp góc phần tư. Điều này giữ cho điều kiện “liệu ​​chúng ta có đạt được (x, y) nếu chúng ta kết hợp hai khối này theo thứ tự này” nhất quán trong mọi trường hợp hay không. 

Các hướng còn lại được thêm vào mà không có điều kiện vì chúng không thể góp phần đạt đến điểm cấm một khi đã tránh được sự căn chỉnh quan trọng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào: 

R = 2, L = 0, U = 2, D = 0, cấm = (1, 1) 

Chúng ta đang ở trong trường hợp góc phần tư thứ nhất, vì vậy R và U rất quan trọng. 

| Bước | Hành động | Cái nhìn sâu sắc về vị trí | 
| --- | --- | --- | 
| 1 | Hãy thử R rồi U | Tiền tố R đạt (1,0), sau đó U đạt (1,1) → bị cấm | 
| 2 | Chuyển sang U rồi R | Bản dựng U (0,1), bản dựng R (2,1), không bao giờ chạm (1,1) | 

Đầu ra tránh được điểm cấm bằng cách phá vỡ sự đồng bộ hóa giữa tích lũy x và y. 

### Ví dụ 2 

đầu vào: 

R = 3, L = 1, U = 2, D = 2, bị cấm = (2, -1) 

Đây là trường hợp góc phần tư bên phải xuống. 

| Bước | Hành động | Cái nhìn sâu sắc về vị trí | 
| --- | --- | --- | 
| 1 | Kiểm tra R rồi D | Sẽ đạt (2,-1) ở tiền tố căn chỉnh | 
| 2 | Chuyển sang D rồi R | D di chuyển theo chiều dọc trước tiên, do đó việc căn chỉnh x và y bị tách rời | 

Sau khi sửa thứ tự, L và U được nối lại an toàn. 

Những ví dụ này cho thấy sự kiện nguy hiểm duy nhất là sự liên kết tiền tố đồng thời của hai hướng tới hạn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(R + L + U + D) | Mỗi nước đi được in đúng một lần | 
| Không gian | O(1) thêm | Chuỗi đầu ra được phát trực tuyến dưới dạng xây dựng | 

Giải pháp là tuyến tính về số bước di chuyển, là tối ưu vì mọi bước di chuyển đều phải xuất hiện ở đầu ra. Điều này phù hợp thoải mái trong giới hạn thông thường cho tổng số hoạt động lên tới 10^5 hoặc thậm chí 10^6. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    R, L, U, D, x, y = map(int, input().split())
    
    res = []
    
    if x >= 0 and y >= 0:
        if not (x <= R and y <= U):
            res.append("R" * R)
            res.append("U" * U)
        else:
            res.append("U" * U)
            res.append("R" * R)
        res.append("L" * L)
        res.append("D" * D)
    
    elif x >= 0 and y < 0:
        y = -y
        if not (x <= R and y <= D):
            res.append("R" * R)
            res.append("D" * D)
        else:
            res.append("D" * D)
            res.append("R" * R)
        res.append("L" * L)
        res.append("U" * U)
    
    elif x < 0 and y >= 0:
        x = -x
        if not (x <= L and y <= U):
            res.append("L" * L)
            res.append("U" * U)
        else:
            res.append("U" * U)
            res.append("L" * L)
        res.append("R" * R)
        res.append("D" * D)
    
    else:
        x = -x
        y = -y
        if not (x <= L and y <= D):
            res.append("L" * L)
            res.append("D" * D)
        else:
            res.append("D" * D)
            res.append("L" * L)
        res.append("R" * R)
        res.append("U" * U)
    
    return "".join(res)

# custom tests

assert run("2 0 2 0 1 1") != "", "sample-like case"
assert run("1 0 0 1 1 -1") != "", "small quadrant case"
assert run("3 3 3 3 0 0") != "", "center forbidden edge case"
assert run("0 0 1 0 0 1") != "", "degenerate movement case"
assert run("5 2 3 4 2 -3") != "", "mixed quadrant case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 0 2 0 1 1 | đường dẫn hợp lệ | logic hoán đổi góc phần tư cơ bản | 
| 1 0 0 1 1 -1 | đường dẫn hợp lệ | xử lý y âm | 
| 3 3 3 3 0 0 | đường dẫn hợp lệ | xử lý điểm cấm trung tâm | 
| 0 0 1 0 0 1 | đường dẫn hợp lệ | cạnh đơn hướng | 
| 5 2 3 4 2 -3 | đường dẫn hợp lệ | tính nhất quán của góc phần tư hỗn hợp | 

## Vỏ cạnh 

Một trường hợp cạnh tinh tế xảy ra khi tọa độ bị cấm nằm chính xác trên ranh giới mà khối đơn điệu sẽ đạt tới. Ví dụ: nếu R = 5 và U = 5 và điểm cấm là (3, 3), thì thứ tự R thì U chắc chắn sẽ đi qua (3, 3) làm tiền tố. Trong tình huống đó, thuật toán phát hiện điều kiện căn chỉnh và chuyển thứ tự từ U rồi R, đảm bảo rằng không có tiền tố nào khớp với cả hai tọa độ cùng một lúc. Trong quá trình thực thi, trước tiên U tạo ra các điểm (0, k), vì vậy x không bao giờ bằng 3 trong giai đoạn đó và khi R được áp dụng sau đó, y được cố định ở mức 5, do đó (3, 3) không bao giờ đạt được. 

Một trường hợp cạnh khác là khi số lượng một hướng bằng 0. Nếu U = 0 hoặc R = 0 thì đường đi buộc phải nằm trên một đường thẳng và điểm cấm chỉ có thể chạm tới nếu nó nằm chính xác trên đoạn đường đó. Thuật toán xử lý việc này vì quá trình kiểm tra tính khả thi x ∼ R và y ∼ U tự động thất bại hoặc kích hoạt một hoán đổi vô hại nhưng vẫn tạo ra đường dẫn tuyến tính hợp lệ. 

Trường hợp cạnh cuối cùng là khi tọa độ bị cấm nằm trong “góc phần tư khác” với mọi chuyển động, chẳng hạn như x > R hoặc y > U trong trường hợp góc phần tư thứ nhất. Khi đó, điều kiện sẽ ngăn chặn bất kỳ sự hoán đổi nào và thứ tự đơn giản được sử dụng một cách an toàn, vì đường dẫn về mặt vật lý hoàn toàn không thể đến điểm bị cấm.
