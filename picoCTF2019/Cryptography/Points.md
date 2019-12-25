# Points - 100 points
Cryptography

## Description:
> Cryptography can be easy, do you know what ROT13 is? cvpbPGS{abg_gbb_onq_bs_n_ceboyrz}

## Hint:
> This can be solved online if you don't want to do it by hand!

## Solving:

As the description suggests this is a simple ROT13 or Caesar cipher that I have a quick bash script to decrypt. (This will ignore any punctuation or numbers and produce all lowercase).
This could have been done online in a second also.
```c
#!/bin/sh
read -p "Enter encoded caesar cipher: " message
read -p "How many iterations do you want to force? " number
message=${message#*"{"}
message=${message%"}"}
IN="$message" 

prefix="picoCTF{"
suffix="}"
echo "\nconverting...\n\n================\n$message\n================\n\n"

for I in $(seq $number); do
    printf '+'$I; printf ': '; printf $prefix; echo $IN$suffix | tr $(printf %${I}s | tr ' ' '.')\a-z a-za-z
done
```
```c
Enter encoded caesar cipher: cvpbPGS{abg_gbb_onq_bs_n_ceboyrz}
How many iterations do you want to force? 13

converting...

================
abg_gbb_onq_bs_n_ceboyrz
================


+1: picoCTF{bch_hcc_por_ct_o_dfcpzsa}
+2: picoCTF{cdi_idd_qps_du_p_egdqatb}
+3: picoCTF{dej_jee_rqt_ev_q_fherbuc}
+4: picoCTF{efk_kff_sru_fw_r_gifscvd}
+5: picoCTF{fgl_lgg_tsv_gx_s_hjgtdwe}
+6: picoCTF{ghm_mhh_utw_hy_t_ikhuexf}
+7: picoCTF{hin_nii_vux_iz_u_jlivfyg}
+8: picoCTF{ijo_ojj_wvy_ja_v_kmjwgzh}
+9: picoCTF{jkp_pkk_xwz_kb_w_lnkxhai}
+10: picoCTF{klq_qll_yxa_lc_x_molyibj}
+11: picoCTF{lmr_rmm_zyb_md_y_npmzjck}
+12: picoCTF{mns_snn_azc_ne_z_oqnakdl}
+13: picoCTF{not_too_bad_of_a_problem}

```

### Flag: 

```c
picoctf{not_too_bad_of_a_problem} 
```
