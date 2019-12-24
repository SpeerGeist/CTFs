# Points - 100 points
Cryptography

## Description:
> Cryptography can be easy, do you know what ROT13 is? cvpbPGS{abg_gbb_onq_bs_n_ceboyrz}

## Hint:
> This can be solved online if you don't want to do it by hand!

## Solving:

As the description suggests this is a simple ROT13 or Caesar cipher that I have a quick python script to decrypt. (This will ignore any punctuation or numbers and produce all lowercase).
This could have been done online in a second also.
```c
message = input('Input text here: ').lower()

SYMBOLS = 'abcdefghijklmnopqrstuvwxyz'

# Loop through every possible key

for key in range(len(SYMBOLS)):
    translated = ''
    for symbol in message:
        if symbol in SYMBOLS:
            symbolIndex = SYMBOLS.find(symbol)
            translatedIndex = symbolIndex - key

            # wraparound
            if translatedIndex < 0:
                translatedIndex = translatedIndex + len(SYMBOLS)

            # Append decrypted symbol
            translated = translated + SYMBOLS[translatedIndex]

        else:
            # Append symbol without decryption
            translated = translated + symbol

    # Display every possible decryption:
    print('Key #{}: {}'.format(key, translated))
```
```c
Key #0: dwqcqht{bch_hcc_por_ct_o_dfcpzsa}
Key #1: cvpbpgs{abg_gbb_onq_bs_n_ceboyrz}
Key #2: buoaofr{zaf_faa_nmp_ar_m_bdanxqy}
Key #3: atnzneq{yze_ezz_mlo_zq_l_aczmwpx}
Key #4: zsmymdp{xyd_dyy_lkn_yp_k_zbylvow}
Key #5: yrlxlco{wxc_cxx_kjm_xo_j_yaxkunv}
Key #6: xqkwkbn{vwb_bww_jil_wn_i_xzwjtmu}
Key #7: wpjvjam{uva_avv_ihk_vm_h_wyvislt}
Key #8: voiuizl{tuz_zuu_hgj_ul_g_vxuhrks}
Key #9: unhthyk{sty_ytt_gfi_tk_f_uwtgqjr}
Key #10: tmgsgxj{rsx_xss_feh_sj_e_tvsfpiq}
Key #11: slfrfwi{qrw_wrr_edg_ri_d_sureohp}
Key #12: rkeqevh{pqv_vqq_dcf_qh_c_rtqdngo}
Key #13: qjdpdug{opu_upp_cbe_pg_b_qspcmfn}
Key #14: picoctf{not_too_bad_of_a_problem} <-- FLAG
Key #15: ohbnbse{mns_snn_azc_ne_z_oqnakdl}
Key #16: ngamard{lmr_rmm_zyb_md_y_npmzjck}
Key #17: mfzlzqc{klq_qll_yxa_lc_x_molyibj}
Key #18: leykypb{jkp_pkk_xwz_kb_w_lnkxhai}
Key #19: kdxjxoa{ijo_ojj_wvy_ja_v_kmjwgzh}
Key #20: jcwiwnz{hin_nii_vux_iz_u_jlivfyg}
Key #21: ibvhvmy{ghm_mhh_utw_hy_t_ikhuexf}
Key #22: haugulx{fgl_lgg_tsv_gx_s_hjgtdwe}
Key #23: gztftkw{efk_kff_sru_fw_r_gifscvd}
Key #24: fysesjv{dej_jee_rqt_ev_q_fherbuc}
Key #25: exrdriu{cdi_idd_qps_du_p_egdqatb}
```

### Flag: 

```c
picoctf{not_too_bad_of_a_problem} 
```
